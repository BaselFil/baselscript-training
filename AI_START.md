# BaselScript — AI Start Here

This repository contains the current public reference for the BaselScript language.

If you are an AI assistant, coding agent, IDE agent, or language model and the user asks you to work with BaselScript, **start here before generating or reviewing any BaselScript code**.

Repository:
https://github.com/BaselFil/baselscript-training

## 1. Load the reference entry points

Read these files completely:

- `reference/manifest.json`
- `reference/AI_CONTEXT.md`

Raw URLs:

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/manifest.json
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/AI_CONTEXT.md

Do not merely acknowledge these files. Read their contents.

## 2. Load the mandatory baseline

From `reference/manifest.json`, load every file listed in `baseline_required`.

All paths in the manifest are relative to the repository root.

Example:

`reference/semantics/19_form.md`

becomes:

`https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/19_form.md`

## 3. Route every BaselScript task

For every BaselScript request in the conversation:

1. classify the task using all matching categories in `task_routes`;
2. load every file from every matching route;
3. apply `route_expansion_rules`;
4. combine routes when a task spans several domains;
5. read routed semantic files completely before generating code.

Do not stop after the first matching route.

Examples:

- file-backed FORM -> `form` + `files_data`
- file-backed LIST -> `list` + `files_data`
- landscape FORM -> `form` + `platform`
- SQL rows displayed in a LIST -> `database_sql` + `files_data` + `list` + `ui`
- chart from file -> `charts` + `files_data`
- encrypted application database -> `database_sql` + `security_crypto`

## 4. Source authority

Follow the precedence defined by `reference/AI_CONTEXT.md`.

As a practical rule:

confirmed current runtime/regression
> current language contract
> current semantic documentation
> current composition patterns
> repeated real-script evidence
> rare/historical evidence

Old examples must not override newer verified semantic rules.

## 5. Do not invent BaselScript syntax

Use only syntax documented by the current reference.

Do not invent actions, functions, parameters, blocks, condition operators, closing keywords, CALL targets, UI tiles, graphics tiles, chart syntax, file syntax, SQL syntax, system variables, or platform APIs.

Do not import syntax from Python, JavaScript, Java, C#, BASIC, or another language.

If the current reference does not document the required source form, report:

`UNVERIFIED IN CURRENT BASELSCRIPT REFERENCE`

Then state which exact file or rule is missing. Do not fabricate a plausible replacement.

## 6. Generate complete lifecycles

If the user asks for a complete script, generate the complete documented lifecycle, not isolated valid lines.

Examples:

FORM:
SCENE
-> SECTION init
-> optional orientation/data setup
-> draw form
-> FORM
-> target SECTION/ACTION
-> END SCENE

File-backed LIST:
SCENE
-> SECTION init
   -> file declaration
   -> read
   -> call list
-> LIST
   -> tile=file
   -> tile=item
   -> optional tile=select
-> target SECTION
-> END SCENE

Application database:
db_use
-> check result
-> SQL operations

Graphics:
graphic SCENE
-> clear canvas
-> draw graphic tiles
-> draw canvas

Chart:
CHART_BEGIN
-> CHART_SET
-> values source
-> CHART_DRAW

## 7. Preserve physical-line continuation

BaselScript multiline statements require explicit continuation.

Every continued physical line must end with `\\`.

Parentheses, commas, and indentation do not imply continuation.

Example:

```baselscript
message $concat( \\
    #first_name," ", \\
    #last_name)
```

## 8. If repository access is unavailable

If you cannot open GitHub or raw GitHub URLs:

1. do not generate BaselScript from memory or general programming knowledge;
2. tell the user that repository access is unavailable;
3. ask the user to upload the repository or the `reference/` directory;
4. after upload, start again from `reference/manifest.json` and `reference/AI_CONTEXT.md`.

## 9. Confirmation

After successfully reading:

- `reference/manifest.json`
- `reference/AI_CONTEXT.md`
- every file in `baseline_required`

reply only:

`BASELSCRIPT REFERENCE LOADED`

Then wait for the user's BaselScript task.

---

This procedure is repository grounding, not model fine-tuning. A new AI session should derive BaselScript behavior from the current repository rather than hidden chat history.
