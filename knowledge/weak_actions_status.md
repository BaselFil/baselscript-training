# BaselScript Weak-Contract Actions Status

## Purpose

This file records actions that were weakly documented in the corpus and were checked against the
current interpreter during the 2026-08-30 training audit.

A name is not CURRENT merely because it once appeared in source code or an old `.def` file.

## Runtime-confirmed CURRENT actions

The following actions were executed or directly confirmed in the current runtime.

### delay

Status: CURRENT / runtime confirmed.

```baselscript
delay 1
```

The maintainer observed the expected pause during `W02_TRACE_DELAY.script`.

### trace_full

Status: CURRENT / runtime confirmed.

```baselscript
trace_full "text"
```

### trace_as_is

Status: CURRENT / runtime confirmed.

```baselscript
trace_as_is "text"
```

### trace_asis

Status: CURRENT alias / runtime confirmed.

```baselscript
trace_asis "text"
```

For new generated code prefer `trace_as_is`.

### parsing

Status: CURRENT / runtime confirmed for hash-array mode.

```baselscript
parsing hash_array=1 string=#source delimiter="="
```

The confirmed regression uses hash array `1`. Do not invent arbitrary hash-array identifiers without
matching runtime evidence.

### remove

Status: CURRENT / runtime confirmed for hash-array key removal.

```baselscript
remove hash_array=1 key=b
```

### update_all_records

Status: CURRENT / runtime confirmed.

`W04_UPDATE_ALL_RECORDS.script` produced:

```text
W04 OK=1 RECORDS=2 AGE11=1 AGE21=1
```

### replace

Status: CURRENT / runtime confirmed for file mode.

Confirmed parameter spellings:

```text
inp_file=
out_file=
inp_dir=
out_dir=
inp_string=
out_string=
```

`W05_REPLACE_FILE.script` produced:

```text
W05 OK=1 VALUE=OMEGA
```

### search_dir

Status: CURRENT / runtime confirmed on Windows.

The maintainer test returned a valid `#_SELECTED_DIRECTORY`.

This remains platform-sensitive; do not assume the same search semantics on Android.

### thumbnail

Status: CURRENT / runtime confirmed.

The current definitions expose `thumb` and `create_thumb` as Prefix families. The real runtime accepts
the full `thumbnail` spelling, and the maintainer confirmed that an output PNG was physically created.

Observed working shape:

```baselscript
thumbnail i_d=#_directory_images i_f=55.jpg \
    o_d=#_directory_thumbnails o_f=weak_test_thumbnail.png w=65 h=65
```

For training, `thumbnail` is the preferred full spelling when using this runtime family.

### message_short

Status: CURRENT / runtime confirmed.

### echo

Status: CURRENT / runtime confirmed.

`M04_MESSAGE_SHORT_ECHO.script` was confirmed OK by the maintainer.

### close

Status: CURRENT / runtime handler confirmed.

Current Windows dispatcher behavior:

```csharp
if (action.Equals("close"))
{
    Config.SubWindow.Close();
    return "continue";
}
```

Language meaning: close the current BaselScript subwindow. Do not use it for file handles,
database connections, or application exit.

### driver_list

Status: CURRENT / runtime handler confirmed.

The current dispatcher calls:

```csharp
Actions.DriverList(content);
```

The exact parameter/result contract is still weakly documented. Recognize the action, but do not
invent parameters until a canonical runtime example is added.

## CURRENT but still weak-contract

These names remain in the current `actions.def`, but their exact parameter contract is not fully
formalized by the current training evidence:

```text
insert_before
insert_after
insert_line_before
insert_line_after
reduce_gups
```

Preserve an existing working pattern when maintaining code. Do not invent parameter names.

## Removed from CURRENT

The following standalone actions are intentionally absent from the current `actions.def` and must
not be generated:

```text
db_current
db_path
db_exists
open
grid
pdf
mail
vibrate
```

Reasons established during the audit:

- `db_current` failed in the current interpreter and the database metadata trio is not part of the
  actively used language.
- `open` produced an `unknown error`; use the confirmed standalone `select` workflow for file
  selection instead.
- `grid`, `pdf`, and `mail` were rejected by the maintainer as non-current language commands.
- standalone `vibrate` is a Windows no-op and no separate Android action was confirmed.

Android vibration remains available as part of notification behavior where the notification record
has vibration enabled. That does not make standalone `vibrate` a CURRENT action.

## AI rule

When an action is absent from the current `language/actions.def`, do not generate it because it
appears in an older source branch, old corpus entry, or historical document.
