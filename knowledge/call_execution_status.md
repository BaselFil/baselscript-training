# BaselScript Call and Execution Status

## Purpose

This file documents CURRENT BaselScript composition and execution patterns for AI generation,
review, validation support, and maintenance.

BaselScript applications are composed from scenes, callable SECTION/ACTION blocks, UI blocks,
and external script transitions. `call` is therefore a central architectural command, not a
single-purpose function call.

## Authority and evidence

The current `actions.def` defines:

```text
call;Exact;CallTarget;None;<type>=<name>
```

Regression coverage confirms:

- `call section=<name>`
- `call sec=<name>`
- `call sec=<name>` targeting an ACTION in the same callable namespace
- `call scene=<name>`
- missing internal SECTION/ACTION detection
- `call scr=<script>`
- `call script=<script>`
- runtime handling of missing external scripts

The real V3 corpus contains extensive usage of:

```text
call scene=...
call script=...
call scr=...
call section=...
call sec=...
call menu=...
call list=...
call dialog=...
```

## Callable namespace

SECTION and ACTION share the same runtime callable namespace.

Canonical form:

```baselscript
SECTION init
    call section=save
END

ACTION save
    message "saved"
END
```

The short alias is also CURRENT:

```baselscript
call sec=save
```

A regression-confirmed compatibility rule allows `sec=` to target an ACTION declaration.
Do not assume SECTION and ACTION are separate function namespaces.

For new code prefer `section=` or `sec=` consistently within a project. Do not invent a new
callable block type.

## Current-scene resolution

A SECTION/ACTION call normally resolves within the current scene.

The validator checks that a statically known SECTION/ACTION name exists. For backward
compatibility, if the same callable name exists elsewhere in the script but not in the current
scene, the validator may avoid a blocking error because historical runtime behavior can silently
ignore that cross-scene mismatch.

AI rule:

- do not intentionally generate a SECTION/ACTION call that depends on a declaration in another scene;
- call the correct scene explicitly, or place the callable in the scene where it is used;
- treat cross-scene callable-name tolerance as legacy compatibility, not a design pattern.

## SCENE calls

Canonical form:

```baselscript
SCENE=1
SECTION init
    call scene=second
END
END SCENE

SCENE=second
SECTION init
    message "second scene"
END
END SCENE
```

`call scene=<name>` is statically checkable when the target is literal. The validator should
report a missing literal scene in the same script.

The runtime also supports a scene call with an explicit target section in existing code:

```baselscript
call scene=<scene> section=<section>
```

Use this form only when the target section is known to exist and the current application pattern
already relies on it.

## UI block calls

CURRENT UI invocation families include:

```baselscript
call list=<name>
call menu=<name>
call dialog=<name>
```

These refer to UI blocks in the current scene and are statically checkable when the names are
literal.

Do not replace these with invented APIs such as `show_list(...)`, `open_menu(...)`, or generic
widget methods.

A `call menu=<name>` target may be a runtime-created MENU established earlier in the same scene:

```baselscript
clear menu=m2
create tile=title text="menu 2"
create tile=item1 text="Back" sec=back
call menu=m2
```

This target is statically knowable even without a `MENU m2` declaration. Validator symbol
collection must therefore include literal runtime MENU names created by `clear menu=...` or the
corresponding runtime-creation form.

## External script calls

Canonical long and short forms:

```baselscript
call script=_setting_colorscheme
call scr=000_templates
```

The runtime treats `script=` and `scr=` as the same external-script family.

Existing scripts also use optional scene/section targets:

```baselscript
call script=sql_notes scene=show_notes section=reload_list
call scr=call_scriptB scene=2
```

Runtime defaults are effectively:

```text
scene=1
section=init
```

when a target is not supplied.

External script references are intentionally not fully proven by the static validator because a
single script cannot establish whether another application file exists at runtime. A missing
literal external script can therefore pass validation and fail only when that path executes.

AI rule:

- do not claim that Validator proves external script existence;
- when modifying a project, verify external targets against the actual application file set;
- preserve existing `script=` vs `scr=` style unless normalization is explicitly requested.

## Dynamic calls

The Windows runtime contains support for resolving a `call` target from a variable before normal
call dispatch. Static reference validation cannot prove such a target.

Dynamic call forms are not common in the V3 corpus. Therefore:

- treat dynamic CALL as runtime-resolved;
- do not generate it by default;
- prefer a literal call target when the target is known statically.

## Runtime integrations through CALL

Real scripts contain special CALL integrations such as:

```baselscript
call site=#link
call google=navigation address=#address
call google=search lat=#lat lng=#lng request=#request
call google=earth file=#file_name dir=APPDIR/files_kml
call voice=#text sec=voice
call speak=#text
```

These are runtime/platform integrations, not references to SECTION/LIST/MENU/etc. `google`,
`voice`, and `speak` are explicitly excluded from normal static script-reference checking.

`site=` is treated as an external target and is not statically proven.

Do not infer new integration names from the generic `<type>=<name>` call syntax.

## Interaction with loops

Confirmed loop-control work preserves the parent loop context across CALL SECTION/ACTION when the
called section starts a nested loop.

Important limitation in the current documented V1 behavior: do not assume arbitrary BREAK or
CONTINUE propagation through any CALL boundary unless a matching regression test confirms it.

## Validation model

For a literal call target:

```text
internal SCENE          -> static reference check
internal SECTION/ACTION -> static reference check with legacy compatibility
internal LIST/MENU/DIALOG -> static reference check
external SCRIPT/SCR     -> runtime existence check
SITE                    -> runtime/external
GOOGLE/VOICE/SPEAK      -> special runtime integration
variable target         -> runtime-resolved
```

## AI generation rules

1. Prefer literal targets when known.
2. Use SECTION/ACTION as one callable namespace.
3. Do not intentionally call a SECTION/ACTION from the wrong scene.
4. Do not report external script existence as statically guaranteed.
5. Preserve `script=`/`scr=` and `section=`/`sec=` aliases when maintaining existing code.
6. Do not invent CALL target families merely because `<type>=<name>` is generic.
7. Check the project file set before renaming an external script target.
8. Treat special platform CALL integrations separately from ordinary script references.
