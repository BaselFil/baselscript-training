# BaselScript Security and Crypto Status

## Purpose

This file documents the CURRENT BaselScript security and cryptography model used by real
applications, especially EXPENSA and MID.

Security commands must be generated conservatively. Do not replace them with invented
cryptography, ad-hoc SQL, plaintext password storage, or assumptions from other languages.

## Two different security layers

BaselScript currently uses two distinct security mechanisms:

1. `CRYPTO_*` - encryption/decryption and application data rekey/filter operations.
2. `SQL_SECURITY_*` - interpreter/system security state such as access keys and protected
   script registration.

Do not merge these models.

`db_use` selects the application database context. `SQL_SECURITY_*` actions are defined as
SystemDatabase operations and must not be treated as normal application-table SQL.

## CURRENT crypto actions

Current formal actions and real application evidence include:

```text
CRYPTO_ENCRYPT
CRYPTO_DECRYPT
CRYPTO_FILTER_CSV
CRYPTO_REKEY_DATABASE
```

### CRYPTO_ENCRYPT

Formal contract:

```baselscript
CRYPTO_ENCRYPT value=<value> key=<key> [OUTPUT(<variable>)]
```

Canonical MID example:

```baselscript
CRYPTO_ENCRYPT \
    value=#master_check_plain \
    key=#_mid_master_password \
    OUTPUT(#master_check_cipher)
```

After execution, real MID code checks:

```baselscript
if #_crypto_result != "1"
    message #_crypto_error
    return
endif
```

Do not assume that a non-empty output variable alone means encryption succeeded.

### CRYPTO_DECRYPT

Formal contract:

```baselscript
CRYPTO_DECRYPT value=<value> key=<key> [OUTPUT(<variable>)]
```

Canonical examples:

```baselscript
CRYPTO_DECRYPT value=#selected_title_cipher key=#_mid_master_password OUTPUT(#edit_title)
```

or multi-line:

```baselscript
CRYPTO_DECRYPT \
    value=#master_check_cipher \
    key=#_mid_master_password \
    OUTPUT(#master_check_plain)
```

Real application logic checks `#_crypto_result` and clears sensitive temporary values on
failure.

## Master-password validation pattern

MID does not store the master password. Instead it stores an encrypted known check value in
the application database and tests whether the entered password can decrypt it correctly.

Canonical model:

```text
known plaintext -> encrypt with entered master password -> store ciphertext
later:
stored ciphertext -> decrypt with entered master password -> compare with known plaintext
```

Real MID check value:

```text
MID_MASTER_CHECK_V1
```

The application first checks whether a `master_password_check` row exists in `system_data`.
If not, it creates the encrypted check row. If it exists, it decrypts the stored ciphertext
and compares the plaintext with the known check string.

This is application-level master-password validation and is not the same mechanism as
`SQL_SECURITY_*` access keys.

Generation rules:

- Never store `#_mid_master_password` in plaintext in SQL.
- Do not store the known check plaintext as the password.
- Store only its encrypted value.
- On failed decrypt or failed comparison, clear the password and temporary plaintext/cipher
  variables when following the MID pattern.
- Preserve the current database schema and key names when modifying MID.

## CRYPTO_FILTER_CSV

Real MID search uses encrypted CSV filtering:

```baselscript
CRYPTO_FILTER_CSV input=MID_TEMP.csv output=MID_RESULT.csv \
    dir=#_directory_files key=#_mid_master_password \
    query=#mid_search_query columns=#mid_search_columns
```

Real result variables:

```text
#_crypto_filter_result
#_crypto_filter_error
```

MID checks:

```baselscript
if #_crypto_filter_result != "1"
    message #_crypto_filter_error
    return
endif
```

Observed parameters:

```text
input
output
dir
key
query
columns
```

Do not infer additional filtering parameters without runtime evidence.

## CRYPTO_REKEY_DATABASE

MID uses this action when changing the master password.

Canonical real pattern:

```baselscript
CRYPTO_REKEY_DATABASE database="MID" table="entries" id="id" \
    columns="title_cipher,username_cipher,password_cipher,url_cipher,address_cipher,note_cipher" \
    old_key=#_mid_master_password new_key=#mid_new_master_password \
    check_column="title_cipher" check_plain="MID_MASTER_CHECK_V1" \
    dir=#_directory_files
```

A second call rekeys `system_data`:

```baselscript
CRYPTO_REKEY_DATABASE database="MID" table="system_data" id="id" \
    columns="value_cipher" \
    old_key=#_mid_master_password new_key=#mid_new_master_password \
    check_column="value_cipher" check_plain="MID_MASTER_CHECK_V1" \
    dir=#_directory_files
```

Real result variables include:

```text
#_crypto_rekey_result
#_crypto_rekey_count
```

MID performs a database backup before rekeying and has rollback logic if rekey fails.
Do not generate a master-password change that merely changes the password variable while
leaving existing encrypted data under the old key.

## CURRENT SQL_SECURITY actions

Current formal SystemDatabase action family:

```text
SQL_SECURITY_ADD_PROTECTED_SCRIPT
SQL_SECURITY_SET_PROTECTED_SCRIPTS
SQL_SECURITY_INIT_PROTECTED_SCRIPTS
SQL_SECURITY_IS_UNLOCKED
SQL_SECURITY_NEEDS_ACCESS_KEY
SQL_SECURITY_ACCESS_ENABLED
SQL_SECURITY_VERIFY_ACCESS_KEY
SQL_SECURITY_SET_ACCESS_KEY
SQL_SECURITY_CHANGE_ACCESS_KEY
SQL_SECURITY_DISABLE_ACCESS_KEY
```

Not every formal action has equally strong corpus examples. Prefer the real EXPENSA/MID
patterns below when generating.

## Access-key flow

EXPENSA uses the interpreter/system access-key mechanism at startup.

Check whether an access key is required:

```baselscript
SQL_SECURITY_NEEDS_ACCESS_KEY OUTPUT(#_access_required)
```

Verify entered key:

```baselscript
SQL_SECURITY_VERIFY_ACCESS_KEY key=#_entered_access_key OUTPUT(#_access_ok)
```

Set a key:

```baselscript
SQL_SECURITY_SET_ACCESS_KEY key=#_new_access_key OUTPUT(#_security_result)
```

Change a key:

```baselscript
SQL_SECURITY_CHANGE_ACCESS_KEY old=#_old_access_key new=#_new_access_key OUTPUT(#_security_result)
```

Disable a key:

```baselscript
SQL_SECURITY_DISABLE_ACCESS_KEY key=#_old_access_key OUTPUT(#_security_result)
```

These access keys are not the MID encryption master password. Do not substitute one for the
other.

## Protected-script registration

Both EXPENSA and MID register groups of scripts under a master application name.

Initialize protected-script state:

```baselscript
SQL_SECURITY_INIT_PROTECTED_SCRIPTS OUTPUT(#_protected_init_result)
```

Register scripts:

```baselscript
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID" OUTPUT(#_protected_add_result)
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID_list" OUTPUT(#_protected_add_result)
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID_editor" OUTPUT(#_protected_add_result)
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID_search" OUTPUT(#_protected_add_result)
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID_support" OUTPUT(#_protected_add_result)
```

EXPENSA uses the same model with `master="EXPENSA"` and its application script family.

Do not register unrelated scripts under an application master without explicit project
requirements.

## Application DB versus System DB

Real application startup may contain:

```baselscript
db_use "EXPENSA"
```

followed later by:

```baselscript
SQL_SECURITY_NEEDS_ACCESS_KEY OUTPUT(#_access_required)
```

This does not mean that `SQL_SECURITY_*` should operate on EXPENSA.db. The formal
`actions.def` classifies the security family as `SystemDatabase` operations. Keep system
security state separate from ordinary application SQL.

Likewise, MID application data encryption uses `db_use "MID"`, normal SQL against MID
application tables, and `CRYPTO_*` for encrypted fields.

## Corpus coverage measured in V3

Leading-command occurrences in the real corpus:

```text
CRYPTO_ENCRYPT                       8
CRYPTO_DECRYPT                      12
CRYPTO_FILTER_CSV                    1
CRYPTO_REKEY_DATABASE                2
SQL_SECURITY_ADD_PROTECTED_SCRIPT   13
SQL_SECURITY_CHANGE_ACCESS_KEY       1
SQL_SECURITY_DISABLE_ACCESS_KEY      1
SQL_SECURITY_INIT_PROTECTED_SCRIPTS  2
SQL_SECURITY_NEEDS_ACCESS_KEY        1
SQL_SECURITY_SET_ACCESS_KEY          1
SQL_SECURITY_VERIFY_ACCESS_KEY       1
```

The corpus also contains formal security actions with little/no direct usage evidence,
including `SQL_SECURITY_IS_UNLOCKED`, `SQL_SECURITY_ACCESS_ENABLED` and
`SQL_SECURITY_SET_PROTECTED_SCRIPTS`. Their action names are CURRENT by definition, but AI
should not invent detailed argument forms where the current formal syntax is still `-`.

## Generation rules

- Keep `CRYPTO_*` and `SQL_SECURITY_*` conceptually separate.
- Respect `SystemDatabase` requirements for `SQL_SECURITY_*`.
- Use `db_use` for the intended application database before application SQL.
- Never store master passwords in plaintext.
- Check crypto result variables after encryption/decryption/rekey/filter operations when
  following the MID patterns.
- Preserve backup/rollback behavior around database-wide rekey operations.
- Do not invent encryption algorithms, key derivation APIs, salts, or parameters that are
  not exposed by BaselScript.
- Do not infer parameter syntax for rarely used formal security actions from their names.
- Preserve protected-script master/script relationships used by the application.
