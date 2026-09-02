# 15 - Network / media / device

Status: PARTIAL / PLATFORM-SENSITIVE

Primary source:

- `language/actions.def`

Current machine-visible families include:

```text
download
upload
music
sound
notif*
thumb*
create_thumb*
CLIPBOARD_SET
driver_list
```

Special CALL integrations also exist:

```baselscript
call site=#link
call google=navigation address=#address
call google=search lat=#lat lng=#lng request=#request
call google=earth file=#file_name dir=APPDIR/files_kml
call voice=#text sec=voice
call speak=#text
```

These are platform/runtime integrations, not structural CALL targets.

## Clipboard

`CLIPBOARD_SET` is CURRENT.

Confirmed project/runtime form:

```baselscript
CLIPBOARD_SET value=#edit_password
```

Confirmed result variables:

```text
#_clipboard_result
#_clipboard_error
```

Typical checked use:

```baselscript
CLIPBOARD_SET value=#edit_password

if #_clipboard_result != "1"
    message #_clipboard_error
    return
endif
```

The current machine contract does not export this parameter syntax by itself, so keep this exact verified form rather than inventing additional clipboard parameters.

## Notifications

`notif*` is a broad Prefix family.

Do not infer arbitrary notification command names from the prefix.

Standalone `vibrate` is intentionally removed from the current action contract.
Notification vibration, where supported, is a notification feature and not proof of a `vibrate` action.

## Media

`music` and `sound` are CURRENT actions.

Functions include media-player duration/position helpers.

Do not invent player objects or methods from another framework.

## Network

`download` and `upload` are CURRENT actions.

Their full endpoint/protocol/authentication semantics are not defined by `actions.def`; use current runtime/project examples.

## Generation rules

- label platform-specific forms explicitly;
- do not invent Android/Windows APIs;
- do not promote historical device actions absent from the current machine contract;
- treat broad Prefix families as incomplete contracts until concrete spellings are verified.
