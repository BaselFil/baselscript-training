# BaselScript Database and SQL Status

## Purpose

This file documents the CURRENT database model and evidence-backed SQL patterns used by
BaselScript applications such as EXPENSA and MID.

Database behavior is architecture-sensitive. Always distinguish the application database
from the system database and obey runtime requirements from `actions.def`.

## Database context

`db_use` selects an application database context.

```baselscript
db_use "EXPENSA"
```

Regression confirms isolation of two application databases: tables created in one selected
database are not silently visible in the other.

For new application SQL, establish the intended database context before SQL actions unless
the surrounding application already guarantees it.

## Database helper actions

CURRENT definitions include:

```text
db_use
db_backup
db_restore
db_current
db_exists
db_path
```

`db_use` has an explicit non-empty parameter requirement in `actions.def`.

Real MID patterns include backup/restore forms such as:

```baselscript
db_backup database="MID" file=#backup_file dir=#_directory_files
```

```baselscript
db_restore database="MID" file=#backup_path
```

For helper actions whose exact parameter grammar is not encoded in the definition file,
copy a confirmed real pattern rather than inventing parameters.

## Application SQL actions

The current language definitions mark application SQL actions with the
`ApplicationDatabase` runtime requirement.

### CREATE TABLE

Confirmed form:

```baselscript
SQL_CREATE_TABLE table(reg_items) \
    fields(id:INTEGER_PRIMARY_KEY,name:TEXT,age:INTEGER)
```

Do not generate the obsolete/broad form `SQL_CREATE`. The validator explicitly protects
against near-miss action names such as `SQL_CREATE_TABL`.

### DROP TABLE

Regression-confirmed:

```baselscript
SQL_DROP_TABLE table(reg_items)
```

The action registry represents DROP as a prefix family, so `SQL_DROP_TABLE` is recognized
through that family. Real older scripts also contain shorter `SQL_DROP table(...)` forms.
For new code, prefer a regression-confirmed form when possible.

### INSERT

Confirmed syntax:

```baselscript
SQL_INSERT INTO reg_items(name,age) VALUES('A',10)
```

Do NOT generate the excluded incorrect form:

```text
SQL_INSERT table(...) FIELDS(...) VALUES(...)
```

The corrected regression specifically established `INTO ... (...) VALUES(...)`.

### UPDATE

Confirmed form:

```baselscript
SQL_UPDATE table(reg_items) \
    FIELDS(age=15) \
    WHERE(name='A')
```

Action names are validated case-insensitively, but prefer the canonical uppercase SQL
spelling in generated code.

### DELETE

Confirmed form:

```baselscript
SQL_DELETE FROM reg_items WHERE(name='B')
```

Real older scripts also contain `SQL_DELETE table(...) WHERE(...)`. Do not normalize a
working legacy application without reason; for new code prefer the regression-confirmed
form.

### SELECT scalar/data

Confirmed scalar SELECT example:

```baselscript
SQL_SELECT age \
    FROM reg_items \
    WHERE name='A' \
    OUTPUT(#age_a)
```

Real application patterns also write query results to files, for example:

```baselscript
SQL_SELECT id, place_name FROM places ORDER BY place_name ASC OUTPUT(#_directory_files,PLACES.csv)
```

Use the surrounding application's established OUTPUT convention.

### COUNT

Confirmed:

```baselscript
SQL_SELECT_COUNT TABLE(reg_items) OUTPUT(#count)
```

### SUM

Regression-confirmed pattern:

```baselscript
SQL_SELECT_SUM table(reg_items) FIELDS(age) OUTPUT(#sum_age)
```

### AVG

The current action registry contains `SQL_SELECT_AVG`, and real corpus examples exist.
Prefer a matching real example when generating it.

### Schema evolution

CURRENT defined actions include:

```baselscript
SQL_ENSURE_COLUMN table(entries) field(entry_type:TEXT)
```

and:

```baselscript
SQL_LIST_COLUMNS table(entries) OUTPUT(#field_names)
```

These are used by MID migration logic and are appropriate for controlled schema evolution.

## ORDER / LIMIT / OFFSET

Windows regression coverage confirms SQL ORDER/LIMIT/OFFSET paths. Preserve the exact form
from the corresponding regression/application example when using these clauses.

## System database security actions

`SQL_SECURITY_*` actions are separate from ordinary application SQL. Their current
definitions require the `SystemDatabase` runtime context where specified.

Defined families include access-key and protected-script operations such as:

```text
SQL_SECURITY_INIT_PROTECTED_SCRIPTS
SQL_SECURITY_ADD_PROTECTED_SCRIPT
SQL_SECURITY_NEEDS_ACCESS_KEY
SQL_SECURITY_VERIFY_ACCESS_KEY
SQL_SECURITY_SET_ACCESS_KEY
SQL_SECURITY_CHANGE_ACCESS_KEY
SQL_SECURITY_DISABLE_ACCESS_KEY
```

Example real patterns:

```baselscript
SQL_SECURITY_INIT_PROTECTED_SCRIPTS OUTPUT(#_protected_init_result)
```

```baselscript
SQL_SECURITY_ADD_PROTECTED_SCRIPT master="MID" script="MID" OUTPUT(#_protected_add_result)
```

```baselscript
SQL_SECURITY_VERIFY_ACCESS_KEY key=#_entered_access_key OUTPUT(#_access_ok)
```

Do not emulate these with application-table SQL and do not assume that selecting an
application DB grants system-database access.

## Corpus-only / unverified SQL spellings

The corpus contains historical SQL actions that are absent from the current `actions.def`:

```text
SQL_SELECT_AGG
SQL_SELECT_DISTINCT
SQL_SELECT_GROUP
```

These are corpus evidence only. Do NOT generate them as CURRENT syntax unless they are
added to the current language registry or independently regression-confirmed.

Case variants such as `SQL_update` / `SQL_delete` are not new actions; action matching is
case-insensitive. Prefer canonical uppercase spelling.

## Database result handling

Real database workflows commonly inspect result/error variables after database selection
or SQL execution. Preserve an existing application's error-handling convention instead of
inventing a new exception model.

BaselScript does not gain TRY/CATCH merely because SQL can fail; TRY/CATCH is not stable
CURRENT syntax.

## Generation rules

- Select or preserve the correct application DB context before application SQL.
- Keep system-security SQL separate from application SQL.
- Prefer regression-confirmed CRUD forms.
- Do not generate `SQL_CREATE` or near-miss SQL action names.
- Do not promote corpus-only `SQL_SELECT_AGG`, `SQL_SELECT_DISTINCT`, or `SQL_SELECT_GROUP` to CURRENT.
- Preserve known output conventions and migration patterns in EXPENSA/MID when editing them.
- Use `SQL_ENSURE_COLUMN` rather than destructive schema recreation when the existing migration design calls for additive evolution.

## Evidence examples

```text
checkpoint/latest_rev19/tests/BATCH11_SQL_CORRECTED/R47C_SQL_CRUD_AGGREGATES.script
checkpoint/latest_rev19/tests/BATCH11_SQL_CORRECTED/R48C_SQL_ORDER_LIMIT_OFFSET.script
checkpoint/latest_rev19/tests/SQL_DB_FILE/R20_TWO_DATABASES_REAL_OK.script
corpus/DATABASE_TWO_DB_TEST.script
corpus/EXPENSA_editor.script
corpus/EXPENSA_places.script
corpus/MID_support.script
```
