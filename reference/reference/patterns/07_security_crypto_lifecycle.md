# Security and crypto lifecycle patterns

Status: CURRENT EXPENSA/MID APPLICATION EVIDENCE + CURRENT CONTRACT

Security has two separate responsibilities in current BaselScript:

```text
SQL_SECURITY_* -> interpreter/system access state
CRYPTO_*       -> application data encryption/decryption/rekey work
```

Do not merge them.

## Protected application access lifecycle

Current EXPENSA-style startup follows this pattern:

```text
SECTION init
-> initialize texts/state
-> SQL_SECURITY_NEEDS_ACCESS_KEY
-> if access not required: continue app readiness
-> otherwise show access-key DIALOG
-> SQL_SECURITY_VERIFY_ACCESS_KEY
-> continue only on success
```

Representative source:

```baselscript
SQL_SECURITY_NEEDS_ACCESS_KEY OUTPUT(#_access_required)

if #_access_required != "1"
    call section=check_app_ready
    return
endif

call dialog=dialog_access_key
```

Verification:

```baselscript
SQL_SECURITY_VERIFY_ACCESS_KEY \
    key=#_entered_access_key \
    OUTPUT(#_access_ok)

if #_access_ok == "1"
    call section=check_app_ready
    return
endif
```

`SQL_SECURITY_*` belongs to SystemDatabase semantics. Do not precede it with `db_use`
as if it were ordinary application-table SQL.

## Encryption/decryption lifecycle

Typical application pattern:

```text
obtain plaintext/ciphertext + key
-> CRYPTO_ENCRYPT or CRYPTO_DECRYPT
-> inspect crypto result state
-> clear or stop on failure
-> continue with application data/UI only on success
```

Representative current form:

```baselscript
CRYPTO_DECRYPT \
    value=#selected_title_cipher \
    key=#_mid_master_password \
    OUTPUT(#edit_title)

if #_crypto_result != "1"
    #edit_title=""
endif
```

## Rekey lifecycle

Database-wide rekey is a transactional application workflow, not a simple UI change.

Current MID practice:

```text
validate new key
-> create database backup
-> stop if backup failed
-> run CRYPTO_REKEY_DATABASE
-> verify result
-> restore backup on failure
-> continue only after all protected data is consistently rekeyed
```

Do not generate a password-change workflow that only changes metadata while leaving
encrypted rows under the old key.

## Protected-script registration

Current application support scripts register related script families under an
application master. Use only documented `SQL_SECURITY_*` registration actions.

Do not register unrelated scripts under an application master by inference.

## Generation rule

For security code:

- load `12_security_crypto.md` and `actions.def`;
- keep SystemDatabase and ApplicationDatabase separate;
- check documented result variables;
- preserve rollback/backup steps around destructive or database-wide crypto work;
- do not invent algorithms, salts, KDF parameters or crypto APIs that BaselScript does
  not expose.
