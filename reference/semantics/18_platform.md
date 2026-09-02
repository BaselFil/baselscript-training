# 18 - Platform

Status: PARTIAL, WITH VERIFIED CROSS-PLATFORM SELECT WORKFLOW

BaselScript shares one language contract across platforms, but some runtime/UI/device behavior is platform-specific.

## Shared contract

The canonical language machine contract is:

```text
actions.def
functions.def
blocks.def
conditions.def
scene.def
baselscript-language.json
```

Windows and Android tooling should consume the same language contract rather than maintain divergent syntax lists.

## Windows

The generated `baselscript-language.json` currently contains Windows UI defaults for:

```text
portrait
landscape
```

with FORM/LIST/MENU/DIALOG fragment dimensions and presentation defaults.

These defaults are layout metadata, not universal source grammar.

## Android

Android-specific storage, picker, notification, Google integration and device behavior must be documented explicitly.

Do not infer Android semantics from Windows UI defaults.

## File selection

Standalone:

```baselscript
select
```

is CURRENT on both Windows and Android.

After successful selection the current workflow populates:

```text
#_SELECTED_FILE
#_SELECTED_DIRECTORY
```

### Windows

Confirmed behavior:

- parser accepts bare `select`;
- native file picker opens;
- selected file/directory values are exposed through the current selected-file variables.

### Android

Confirmed behavior:

- parser accepts bare `select`;
- Android system document picker opens;
- the selected URI can be read;
- the selected file is copied into BaselScript-accessible `MYDATA/temp`;
- `#_SELECTED_FILE` is populated;
- `#_SELECTED_DIRECTORY` is populated;
- interpreter execution resumes after selection;
- the imported script can be copied into `APPDIR/MYDATA/script` and executed;
- broad all-files access is not required for this workflow.

Do not assume that Android gives BaselScript unrestricted direct filesystem access to the original public-storage path.

Removed standalone `open` must not be reintroduced as a generic picker action.

## Directories

APPDIR/MYDATA and runtime directory variables must be interpreted through the platform runtime.

Do not convert BaselScript paths directly into Windows drive paths or Android filesystem paths unless the routed platform reference confirms that mapping.

## UI differences

A source FORM/LIST/MENU can be shared while rendered size/defaults differ by platform/orientation.

Do not encode Windows pixel defaults as semantic requirements of the language.

## Platform integrations

Forms such as:

```text
call google=...
call voice=...
call speak=...
call site=...
```

are platform/runtime integrations.

Generate them only from verified platform examples.

## Generation rules

- default to common BaselScript syntax when behavior is platform-independent;
- state Windows/Android assumptions when behavior differs;
- never invent platform APIs;
- prefer standalone `select` for the current file-picker workflow;
- if a feature is absent from the current shared contract, do not present a platform-only historical implementation as current language syntax without verification.
