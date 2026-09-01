# Database and SQL lifecycle patterns

Status: CURRENT CONTRACT + CURRENT APP SCRIPTS + REGRESSION/BACKUP WORK

The corpus contains dedicated SQL/DB applications and tests. Current application SQL
must be treated as a lifecycle, not as isolated SQL-like statements.

## Application database gate

Before ordinary application SQL:

```text
db_use
-> check #_database_result
-> stop/return on failure
-> execute application SQL
```

Canonical pattern:

```baselscript
SCENE=1 title="Database example"

SECTION init

    #_database_result="0"
    db_use "PERSONS"

    if #_database_result != "1"
        message #_error
        return
    endif

    SQL_CREATE_TABLE table(persons) \
        fields(id:INTEGER_PRIMARY_KEY,name:TEXT,city:TEXT)

END

END SCENE
```

Do not execute application SQL first and select the database afterwards.

## Initialization lifecycle

Typical application initialization:

```text
db_use
-> verify success
-> create/check root tables
-> ensure required columns
-> optional initial data
-> continue to normal application UI
```

Example:

```baselscript
SQL_CREATE_TABLE table(persons) \
    fields(id:INTEGER_PRIMARY_KEY,name:TEXT,city:TEXT)

SQL_ENSURE_COLUMN table(persons) field(email:TEXT)

SQL_LIST_COLUMNS table(persons) OUTPUT(#person_columns)
```

Use only the SQL forms documented by `11_database_sql.md` and current `actions.def`.

## Insert lifecycle

```baselscript
#name="Peter"
#city="Mannheim"

SQL_INSERT INTO persons(name,city) \
    VALUES(#name,#city)
```

## Read lifecycle

Single/aggregate values can be returned to variables when the documented command
supports it.

Multiple rows are commonly written to a temporary CSV and then consumed through the
file/LIST layer.

Conceptual pipeline:

```text
db_use
-> SQL_SELECT ... OUTPUT(temp CSV)
-> declare/read the result file
-> call list
```

When generating this pipeline, load both `database_sql` and `files_data`, plus LIST/UI
if the result is displayed.

## Update and delete

```baselscript
#new_city="Karlsruhe"

SQL_UPDATE table(persons) \
    FIELDS(city=#new_city) \
    WHERE(id=1)

SQL_DELETE FROM persons \
    WHERE(id=1)
```

Do not omit a required WHERE merely because the user's natural-language request says
"change this record"; preserve record identity explicitly.

## Backup lifecycle

```baselscript
#backup_file="PERSONS_backup.db"

db_backup database="PERSONS" \
    file=#backup_file \
    dir=#_directory_files

if #_database_backup_result != "1"
    message #_error
    return
endif
```

## Restore lifecycle

```baselscript
db_restore database="PERSONS" \
    file="PERSONS_backup.db" \
    dir=#_directory_files \
    required_table="persons"

if #_database_restore_result != "1"
    message #_error
    return
endif
```

Use the documented safety-path/result variables when the task needs diagnostic or
rollback handling.

## ApplicationDatabase versus SystemDatabase

Keep these domains separate:

```text
db_use + normal SQL_*    -> ApplicationDatabase
SQL_SECURITY_*           -> SystemDatabase
```

`SQL_SECURITY_*` is not a substitute for application-table SQL and must not silently
operate on the application database selected by `db_use`.

## Generation rule

A complete database example must show enough lifecycle to prevent accidental execution
against the wrong database. At minimum, application SQL generation should include or
preserve the `db_use` gate when the surrounding script does not already establish it.
