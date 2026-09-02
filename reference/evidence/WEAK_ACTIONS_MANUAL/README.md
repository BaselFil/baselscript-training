# Manual weak-action tests

These scripts are intentionally not part of unattended regression execution.

- `M01_OPEN_FILE_PICKER.script` opens a native UI picker.
- `M02_SEARCH_DIR.script` depends on the local installation directory and a known filename.
- `M03_THUMBNAIL.script` depends on local image files/directories.
- `M04_MESSAGE_SHORT_ECHO.script` opens/uses the runtime message canvas and is therefore manual.

Do not add unattended tests for `mail`, `close`, `grid`, or other external/UI side effects until a
safe deterministic test harness exists.

`pdf` and Windows `vibrate` are runtime stubs/no-ops in the inspected checkpoint and should not be
reported as confirmed working features from that checkpoint.
