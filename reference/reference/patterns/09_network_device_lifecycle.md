# Network, media and device lifecycle patterns

Status: CORPUS-GROUNDED, EXACT SYNTAX DEFERRED TO CURRENT SEMANTICS

Network/device operations are less uniform than core UI, files and CALL. The corpus
contains 49 NETWORK-tagged scripts and 12 MEDIA/DEVICE-tagged scripts, with several
rare parameter combinations.

Therefore this pattern file defines composition responsibilities, not permission to
promote every historical parameter spelling.

## Network operation lifecycle

Typical structure:

```text
prepare source URL / target / options
-> execute current network action
-> route success to a SECTION when supported
-> route failure to documented error state/SECTION
-> continue UI or file processing
```

Observed corpus families include download-to-file/stream, download-to-string and upload
operations. Exact current source forms must come from `15_network_media_device.md` and
`actions.def`.

## Download to structured data

Common cross-domain pipeline:

```text
download data
-> save to a BaselScript-accessible file
-> declare/read structured file
-> process or display through LIST
```

Load both `network_media_device` and `files_data`. If displayed as LIST, also load the
LIST/UI route.

## Download to string

When current semantics confirms a callback form, the lifecycle is:

```text
prepare URL
-> download into target variable
-> success section consumes target
-> error path reads documented network error state
```

Do not invent Promise/async/await syntax.

## Media/device operations

Music, sound, notification, camera and installed-app behavior is platform-sensitive.

Generation rule:

- load `15_network_media_device.md`;
- use only the documented current action and parameters;
- include the UI/event lifecycle when user interaction starts/stops media;
- do not infer Android or Windows APIs from the platform implementation language.

## Removed action reminder

Do not generate standalone:

```text
open
mail
vibrate
```

unless a future current machine contract reintroduces them. Notification vibration is
not evidence for a standalone `vibrate` action.
