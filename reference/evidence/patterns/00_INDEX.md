# BaselScript composition patterns

Status: SECOND-PASS CORPUS-GROUNDED REFERENCE

This directory complements `reference/semantics/`.

The three reference layers have different jobs:

```text
language/*.def + baselscript-language.json
    -> what exists in the current language contract

semantics/*.md
    -> what a construct means and which source forms are verified

patterns/*.md
    -> how verified constructs are composed into working script lifecycles
```

A pattern file is not permission to invent syntax. Exact action names, function names,
parameters, return conventions, system variables, block grammar and platform behavior
still come from the routed machine contract and semantic files.

## Why this layer exists

The BaselScript corpus contains many complete applications and examples, not only
isolated commands. A model that sees only atomic syntax can know that `read`,
`call list`, `tile=file` and `tile=item` exist while still failing to assemble the
complete file-backed LIST lifecycle.

For complete-script requests, prefer a complete canonical lifecycle pattern when one
is available.

## Pattern routes

```text
structure / call execution -> 01_structure_execution.md
UI / FORM / LIST / MENU / DIALOG -> 02_ui_lifecycle.md
files / structured data -> 03_files_data_lifecycle.md
database / SQL -> 04_database_sql_lifecycle.md
arrays / functions -> 05_arrays_functions_lifecycle.md
graphics / charts -> 06_graphics_charts_lifecycle.md
security / crypto -> 07_security_crypto_lifecycle.md
IF / FOR / WHILE / FOREACH / ASSERT -> 08_control_flow_lifecycle.md
network / media / device -> 09_network_device_lifecycle.md
script/meta operations -> 10_script_meta_lifecycle.md
```

## Route-union rule

Many useful BaselScript tasks cross domains.

Examples:

```text
file-backed LIST
    -> UI + LIST + files_data

SQL result displayed as LIST
    -> database_sql + files_data + LIST + UI

chart from file
    -> charts + files_data

encrypted database editor
    -> database_sql + security_crypto + UI + call_execution
```

Load every matching task route. Do not stop after the first matching category.

## Confidence rule

Patterns in this directory are promoted only when they are supported by one or more
of the following:

- current runtime or regression evidence;
- current machine contract plus semantic documentation;
- repeated real-script evidence;
- current application scripts such as EXPENSA or MID.

Rare or historical corpus syntax is not automatically canonical. If an exact source
form is only weakly evidenced, defer to the routed semantic file and machine contract.
