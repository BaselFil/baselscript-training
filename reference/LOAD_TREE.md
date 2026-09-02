# BaselScript load tree - clean v1

This file documents the proposed loading tree. It is not itself baseline model context.

```text
reference/manifest.json
    |
    +-- baseline_required
    |     +-- AI_CONTEXT.md
    |     +-- knowledge/generation_rules.md
    |     +-- semantics/00_INDEX.md
    |     `-- semantics/01_core_language.md
    |
    +-- classify task
    |     +-- form        -> 19_form + UI pattern + small .def files
    |     +-- list        -> 21_list + UI pattern + small .def files
    |     +-- dialog      -> 20_dialog + UI pattern + small .def files
    |     +-- files_data  -> 06_files_data + file lifecycle + actions/functions
    |     +-- ...
    |
    +-- expand cross-domain task
    |     +-- complete_script
    |     +-- file_backed_list
    |     +-- adaptive_form
    |     `-- ...
    |
    `-- optional fallback
          +-- ui_defaults -> baselscript-language.json
          `-- machine_full -> all .def + baselscript-language.json
```

## Example: person file + adaptive input FORM + adaptive LIST

Load:

```text
baseline_required
+ files_data
+ form
+ list
+ platform
+ structure
+ call_execution
```

Do not load database, graphics, charts, crypto, network or the full machine JSON for this request.

## Why the full JSON is not baseline

The current `baselscript-language.json` is about 120 KB and largely duplicates the smaller `.def`
files plus generated UI metadata. Loading it for every question dominates prompt size. It remains
available for exact UI-default lookup and full machine-contract validation.
