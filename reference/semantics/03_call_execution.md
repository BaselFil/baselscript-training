# 03 - CALL / execution

Status: MOSTLY VERIFIED

This file defines CURRENT BaselScript composition and execution patterns around `call`.

Primary machine source:

- `language/actions.def`

Primary semantic evidence:

- current call/execution regression coverage
- current runtime behavior
- current V3 corpus

`call` is a central composition command in BaselScript. It is used for internal callable blocks,
scene transitions, UI blocks, external scripts, and selected runtime/platform integrations.

---

## 1. Machine contract

Status: VERIFIED BY MACHINE CONTRACT

Current `actions.def` defines:

```text
call;Exact;CallTarget;None;<type>=<name>
```

Canonical general form:

```baselscript
call <type>=<name>
```

Important:

The generic machine form does not mean that arbitrary `<type>` values are valid.

Do not invent CALL target families.

---

## 2. Confirmed CALL families

Status: VERIFIED BY REGRESSION / REAL CORPUS

Current confirmed ordinary CALL families include:

```baselscript
call section=<name>
call sec=<name>
call scene=<name>
call list=<name>
call menu=<name>
call dialog=<name>
call script=<script>
call scr=<script>
```

Current real scripts also contain special runtime integrations documented later in this file.

---

## 3. SECTION and ACTION callable namespace

Status: VERIFIED BY REGRESSION / CURRENT VALIDATION SEMANTICS

SECTION and ACTION share the same runtime callable namespace.

Canonical example:

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

A regression-confirmed compatibility rule allows:

```baselscript
call sec=<name>
```

to target an ACTION declaration.

Therefore:

- do not model SECTION and ACTION as separate callable namespaces;
- do not invent a separate function-like invocation mechanism for ACTION;
- use `section=` or `sec=` consistently within a project unless preserving existing style requires otherwise.

---

## 4. Current-scene resolution

Status: VERIFIED WITH LEGACY COMPATIBILITY

A SECTION/ACTION call normally resolves within the current SCENE.

The validator checks a statically known SECTION/ACTION target.

For backward compatibility, if the same callable name exists elsewhere in the script but not in
the current SCENE, the validator may avoid a blocking error because historical runtime behavior
can silently ignore that cross-scene mismatch.

AI generation rule:

- do not intentionally generate a SECTION/ACTION call that depends on a declaration in another SCENE;
- call the correct SCENE explicitly, or place the callable in the SCENE where it is used;
- treat cross-scene callable-name tolerance as legacy compatibility, not as a design pattern.

---

## 5. SCENE calls

Status: VERIFIED BY REGRESSION / REAL CORPUS

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

A literal:

```baselscript
call scene=<name>
```

is statically checkable.

The validator should report a missing literal SCENE target in the same script.

---

## 6. SCENE call with target section

Status: VERIFIED BY CURRENT RUNTIME / EXISTING CODE

Existing code also uses:

```baselscript
call scene=<scene> section=<section>
```

Example form:

```baselscript
call scene=show_notes section=reload_list
```

Use this form only when the target SECTION is known to exist and the application pattern already
relies on explicit SCENE + SECTION targeting.

Do not infer additional SCENE-call parameters from this form.

---

## 7. UI block calls

Status: VERIFIED BY REGRESSION / REAL CORPUS

Current UI invocation families include:

```baselscript
call list=<name>
call menu=<name>
call dialog=<name>
```

These refer to UI blocks in the current SCENE and are statically checkable when their names are
literal and statically knowable.

Do not replace them with invented APIs such as:

```text
show_list(...)
open_menu(...)
show_dialog(...)
```

or generic widget methods.

---

## 8. Runtime-created UI targets

Status: VERIFIED FOR CURRENT DYNAMIC MENU PATTERN

A CALL target may refer to a runtime-created UI object rather than a static declaration.

Confirmed MENU example:

```baselscript
SECTION e1
    clear menu=m2
    create tile=title text="menu 2"
    create tile=item1 text="Back" sec=back
    call menu=m2
END
```

This means:

- `clear menu=m2` establishes the runtime MENU name;
- following `create tile=...` lines populate that MENU;
- `call menu=m2` invokes the runtime-created MENU;
- a static `MENU m2 ... END` declaration is not required for this pattern.

Validator symbol collection must therefore account for statically knowable runtime-created UI names
where that behavior is supported.

Detailed dynamic UI semantics belong to `05_ui.md`.

---

## 9. External script calls

Status: VERIFIED BY REGRESSION / REAL CORPUS

Canonical long and short forms:

```baselscript
call script=_setting_colorscheme
call scr=000_templates
```

The runtime treats:

```text
script=
scr=
```

as the same external-script family.

When maintaining existing code, preserve the working project style unless normalization is
explicitly requested.

---

## 10. External script with SCENE / SECTION target

Status: VERIFIED BY EXISTING CODE

Existing scripts use optional targets such as:

```baselscript
call script=sql_notes scene=show_notes section=reload_list
call scr=call_scriptB scene=2
```

The current documented runtime defaults are effectively:

```text
scene=1
section=init
```

when the corresponding target is not supplied.

Do not invent other default target names.

---

## 11. External script validation boundary

Status: VERIFIED CURRENT VALIDATION MODEL

A single-script static validator cannot fully prove whether an external application script exists
at runtime.

Therefore a missing literal external script may pass static validation and fail only when that path
executes.

AI rule:

- do not claim that Validator proves external script existence;
- when reviewing or modifying a complete project, verify external targets against the actual
  application file set;
- check external references before renaming a script;
- distinguish internal reference validation from external runtime existence.

---

## 12. Dynamic CALL targets

Status: RUNTIME-SUPPORTED / UNCOMMON IN CORPUS

The Windows runtime contains support for resolving a CALL target from a variable before normal
dispatch.

Such a target cannot generally be proven by static reference validation.

Policy:

- treat dynamic CALL as runtime-resolved;
- do not generate it by default;
- prefer a literal target when the target is statically known;
- if maintaining existing dynamic CALL code, preserve it unless the task explicitly requires
  refactoring.

---

## 13. Special runtime integrations through CALL

Status: OBSERVED IN REAL CORPUS / PLATFORM-SPECIFIC

Real scripts contain special forms such as:

```baselscript
call site=#link
call google=navigation address=#address
call google=search lat=#lat lng=#lng request=#request
call google=earth file=#file_name dir=APPDIR/files_kml
call voice=#text sec=voice
call speak=#text
```

These are runtime/platform integrations.

They are not references to SECTION, ACTION, LIST, MENU, DIALOG, or SCENE declarations.

Current validation notes:

- `google`, `voice`, and `speak` are explicitly excluded from normal static script-reference checking;
- `site=` is treated as an external/runtime target and is not statically proven.

Do not infer new integration names from the generic:

```text
call <type>=<name>
```

machine syntax.

Detailed platform behavior belongs to `15_network_media_device.md` and `18_platform.md`.

---

## 14. CALL and loops

Status: PARTIALLY VERIFIED

Confirmed loop-control work preserves the parent loop context across CALL SECTION/ACTION when the
called SECTION starts a nested loop.

Important limitation:

Do not assume arbitrary BREAK or CONTINUE propagation through every CALL boundary unless a matching
regression test confirms that exact behavior.

Loop syntax and control-flow semantics belong to `04_control_flow.md`.

---

## 15. Static/runtime validation matrix

Status: VERIFIED CURRENT MODEL

For a literal target:

```text
internal SCENE             -> static reference check
internal SECTION/ACTION    -> static reference check with legacy compatibility
internal LIST/MENU/DIALOG  -> static reference check
external SCRIPT/SCR        -> runtime existence check
SITE                       -> runtime/external
GOOGLE/VOICE/SPEAK         -> special runtime integration
variable target            -> runtime-resolved
```

This matrix is part of the AI reference contract.

Do not report a runtime-resolved target as statically guaranteed.

---

## 16. Frequency evidence from current corpus

Status: OBSERVED CORPUS EVIDENCE

The current pattern catalog shows CALL as a major BaselScript composition mechanism.

Frequently observed patterns include:

```text
call | scene
call | script
call | scr
call | menu
call | section
call | list
call | dialog
call | sec
```

This corpus frequency supports these families as normal language usage.

Frequency does not by itself define semantics or prove that every historical pattern is CURRENT.

---

## 17. Canonical generation rules

Status: NORMATIVE REFERENCE RULE

1. Prefer literal CALL targets when the target is known.
2. Treat SECTION and ACTION as one callable namespace.
3. Do not intentionally call a SECTION/ACTION from the wrong SCENE.
4. Use explicit `call scene=...` when a scene transition is intended.
5. Use `call list=...`, `call menu=...`, and `call dialog=...` for the confirmed UI families.
6. Do not report external SCRIPT/SCR existence as statically guaranteed.
7. Preserve `script=` / `scr=` and `section=` / `sec=` style when maintaining existing code.
8. Do not invent CALL target families merely because the machine syntax is generic.
9. Check the project file set before renaming an external script target.
10. Treat GOOGLE/VOICE/SPEAK/SITE forms as special runtime/platform integrations.
11. Do not infer arbitrary BREAK/CONTINUE behavior across CALL boundaries.
12. Prefer ordinary literal CALL forms over dynamic variable-resolved CALL when generating new code.

---

## 18. Do not invent CALL syntax

Do not generate forms such as:

```text
call("section", "save")
save()
invoke section=save
execute section=save
goto section=save
show_list(name)
open_menu(name)
```

unless a future verified BaselScript contract explicitly introduces them.

---

## 19. Scope limits

This file intentionally does not define:

- full timer/start/stop execution semantics;
- `run` command semantics;
- `reload` semantics;
- dynamic UI construction beyond the CALL-target consequence;
- full platform behavior of GOOGLE/SITE/VOICE/SPEAK;
- every historical CALL spelling in the corpus;
- arbitrary BREAK/CONTINUE propagation through nested CALL structures.

Those belong to their routed semantic categories or require dedicated regression evidence.
