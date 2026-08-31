# 11 - Database / SQL

Status: STRONGLY GROUNDED FOR CURRENT SQL ACTION FAMILY

Primary sources:

- `language/actions.def`
- `language/functions.def`
- current application/regression patterns

## Database selection

CURRENT action:

```baselscript
db_use <db>
```

Real pattern:

```baselscript
db_use "PERSONS"

if #_database_result != "1"
    message #_error
    return
endif
```

Database selection is context. Do not assume a SQL action automatically targets the intended application database without the correct active database state.

## ApplicationDatabase actions

Current formal family:

```text
SQL_DROP*
SQL_CREATE_TABLE
SQL_ENSURE*
SQL_LIST_COLUMNS*
SQL_INSERT*
SQL_UPDATE*
SQL_DELETE*
SQL_SELECT_SUM*
SQL_SELECT_COUNT*
SQL_SELECT_AVG*
SQL_SELECT*
SQL_EXIST*
```

Examples from the machine syntax:

```baselscript
SQL_CREATE_TABLE table(<t>) fields(<f:type,...>)
SQL_INSERT INTO <t>(<f>) VALUES(<v>)
SQL_UPDATE table(<t>) FIELDS(<a>) [WHERE(...)]
SQL_DELETE FROM <t> [WHERE(...)]
SQL_SELECT <f> FROM <t> [WHERE(...)] [OUTPUT(<x>)]
```

## SELECT output pipeline

Real application pattern:

```baselscript
SQL_SELECT id,first_name,last_name \
    FROM persons \
    ORDER BY id DESC \
    OUTPUT(#_directory_files,PERSONS_TEMP.csv)
```

The resulting CSV can then be mapped/read by the file subsystem.

## Function requirement

`sql_message` is marked `ApplicationDatabase`.

## Separation rule

ApplicationDatabase SQL actions are distinct from `SQL_SECURITY_*`, which belong to SystemDatabase security.

Do not merge these contexts.

## Generation rules

- issue `db_use` when the application pattern requires explicit database selection;
- preserve schema/table/field names exactly;
- do not invent SQL_* actions absent from the machine contract;
- treat rare corpus-only SQL families as unverified unless promoted by runtime/machine evidence;
- do not assume raw SQLite syntax is accepted everywhere that a BaselScript SQL_* action is expected.
