# Runtime-confirmed results

Date: 2026-08-30

Confirmed by maintainer execution in the current Windows interpreter:

```text
W02_TRACE_DELAY            PASS
W03_HASH_PARSING_REMOVE    PASS
W04_UPDATE_ALL_RECORDS     PASS
W05_REPLACE_FILE           PASS
M02_SEARCH_DIR             PASS
M03_THUMBNAIL              PASS + output PNG physically created
M04_MESSAGE_SHORT_ECHO     PASS
```

Negative/removal findings:

```text
db_current                 unknown error -> removed
open                       unknown error -> removed
grid                       maintainer confirms non-current
pdf                        maintainer confirms non-current
mail                       maintainer confirms non-current
vibrate                    standalone action removed
```

Open validator issue:

```text
R03_DYNAMIC_MENU_REAL_OK   runtime language valid, current validator build reports missing MENU
```
