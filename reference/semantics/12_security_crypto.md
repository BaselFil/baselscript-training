# 12 - Security / crypto

Status: STRONGLY GROUNDED FOR CURRENT CRYPTO AND SQL_SECURITY FAMILIES

Primary source:

- `language/actions.def`

## Two different security layers

Keep these mechanisms separate:

```text
CRYPTO_*       -> application data encryption/decryption/rekey/filter
SQL_SECURITY_* -> interpreter/system security state
```

`SQL_SECURITY_*` actions are SystemDatabase operations. Do not treat them as ordinary SQL against the active application database.

## CRYPTO_ENCRYPT / CRYPTO_DECRYPT

Formal syntax:

```baselscript
CRYPTO_ENCRYPT value=<v> key=<k> [OUTPUT(<x>)]
CRYPTO_DECRYPT value=<v> key=<k> [OUTPUT(<x>)]
```

Confirmed project pattern:

```baselscript
CRYPTO_ENCRYPT \
    value=#master_check_plain \
    key=#_mid_master_password \
    OUTPUT(#master_check_cipher)

if #_crypto_result != "1"
    message #_crypto_error
    return
endif
```

The same result convention is used for decrypt:

```text
#_crypto_result
#_crypto_error
```

Do not treat a non-empty output variable alone as proof that crypto succeeded.

## Master-password validation

A confirmed application pattern validates a password by decrypting an encrypted known check value.

Concept:

```text
known plaintext
-> encrypt with entered master password
-> store ciphertext

later:
stored ciphertext
-> decrypt with entered password
-> compare with known plaintext
```

Do not store the actual master password in plaintext.

The application-level master-password pattern is distinct from `SQL_SECURITY_*` access-key management.

## CRYPTO_FILTER_CSV

Confirmed project form:

```baselscript
CRYPTO_FILTER_CSV input=MID_TEMP.csv output=MID_RESULT.csv \
    dir=#_directory_files key=#_mid_master_password \
    query=#mid_search_query columns=#mid_search_columns
```

Result variables:

```text
#_crypto_filter_result
#_crypto_filter_error
```

Do not infer additional parameters without runtime evidence.

## CRYPTO_REKEY_DATABASE

Current action exists and is used for re-encrypting existing database fields under a new key.

Confirmed project behavior includes checking:

```text
#_crypto_rekey_result
#_crypto_rekey_count
#_crypto_rekey_error
```

The maintained MID flow creates a database backup before rekeying and restores that backup if a later rekey stage fails.

A password change must not merely replace a password variable while leaving stored ciphertext under the old key.

## SQL_SECURITY family

Current formal SystemDatabase actions include:

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

Confirmed access-key forms include:

```baselscript
SQL_SECURITY_NEEDS_ACCESS_KEY OUTPUT(#_access_required)
SQL_SECURITY_VERIFY_ACCESS_KEY key=#_entered_access_key OUTPUT(#_access_ok)
SQL_SECURITY_SET_ACCESS_KEY key=#_new_access_key OUTPUT(#_security_result)
SQL_SECURITY_CHANGE_ACCESS_KEY old=#_old_access_key new=#_new_access_key OUTPUT(#_security_result)
SQL_SECURITY_DISABLE_ACCESS_KEY key=#_old_access_key OUTPUT(#_security_result)
```

Confirmed protected-script initialization/registration forms include:

```baselscript
SQL_SECURITY_INIT_PROTECTED_SCRIPTS OUTPUT(#_protected_init_result)
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID" OUTPUT(#_protected_add_result)
```

Not every formal SQL_SECURITY action has equally strong parameter evidence. Do not infer argument layouts for weakly documented members from their names.

## Generation rules

- never expose keys or passwords in logs;
- preserve application backup/rollback patterns around rekeying;
- distinguish application encryption from interpreter/system access-key security;
- keep SystemDatabase and ApplicationDatabase context separate;
- check documented crypto result variables;
- do not invent crypto algorithms, modes, key derivation, salts, or parameters that are not exposed by BaselScript.
