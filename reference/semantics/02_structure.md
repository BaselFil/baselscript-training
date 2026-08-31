# 02 - Structure

Status: MOSTLY VERIFIED

This file defines BaselScript structural source patterns.

Primary machine sources:

- `language/blocks.def`
- `language/scene.def`

Supporting verified sources include current runtime code, regression-backed UI documentation, and current real scripts.

---

## 1. Structural model

A BaselScript script is organized around SCENE blocks.

Inside a SCENE, the current machine contract recognizes these structural block types:

```text
SECTION
ACTION
MENU
LIST
DIALOG
FORM
POPUPMENU
```

SCENE is intentionally defined separately because it has special grammar and implicit-boundary behavior.

---

## 2. Canonical block names

Status: VERIFIED BY MACHINE CONTRACT

The public structural names are:

```text
SECTION
ACTION
MENU
LIST
DIALOG
FORM
POPUPMENU
SCENE
```

Do not generate validator matching tokens as source-level block names.

For example, `blocks.def` contains internal matching tokens such as:

```text
sec
act
dia
```

These are validator/runtime matching metadata.

Generate:

```baselscript
SECTION init
ACTION save
DIALOG confirm
```

Do not generate:

```text
sec init
act save
dia confirm
```

---

## 3. SCENE start

Status: VERIFIED BY MACHINE CONTRACT / REAL SCRIPTS

`scene.def` defines:

```text
type = SCENE
start keyword = scene
value separator = =
name required = true
implicit next start = true
implicit EOF = true
```

Canonical source form:

```baselscript
SCENE=<name>
```

Real scripts also attach scene parameters after the scene identifier.

Examples:

```baselscript
SCENE=1 title="Example"
SCENE=category_editor title=#categories_title
SCENE=analysis_by_month
```

The exact set of optional SCENE parameters is not defined by `scene.def` itself and belongs to UI/platform semantics.

Do not infer extra SCENE parameters from the machine definition alone.

---

## 4. SCENE end

Status: VERIFIED BY MACHINE CONTRACT / CURRENT RUNTIME / REAL SCRIPTS

The explicit scene terminator is:

```baselscript
END SCENE
```

Current runtime handling recognizes a line starting with `end` that contains `scene`.

Real scripts also contain historical forms with a trailing scene identifier, for example:

```baselscript
END SCENE 1
```

For new generated code prefer:

```baselscript
END SCENE
```

unless maintaining a project that consistently uses the historical numbered form.

Do not generate:

```text
ENDSCENE
END_SCENE
```

unless a future current contract explicitly documents such spelling.

---

## 5. Implicit SCENE boundaries

Status: VERIFIED BY MACHINE CONTRACT

`scene.def` declares:

```text
implicit_next_start = true
implicit_eof = true
```

Therefore the scene model has compatibility support for:

- a following SCENE start implicitly ending the previous SCENE;
- end-of-file implicitly ending the current SCENE.

These are compatibility semantics.

AI generation rule:

Prefer explicit:

```baselscript
END SCENE
```

for new code.

Do not intentionally omit `END SCENE` merely because implicit boundaries are accepted.

---

## 6. SECTION

Status: VERIFIED BY MACHINE CONTRACT / REAL SCRIPTS

Canonical form:

```baselscript
SECTION <name>
    ...
END
```

Example:

```baselscript
SECTION init
    message "Hello"
END
```

`SECTION init` is a common scene entry block in current scripts.

The detailed execution semantics of `init` belong to CALL/execution semantics; this file only defines structure.

Do not generate:

```text
ENDSECTION
```

Canonical current style uses the generic block terminator:

```text
END
```

---

## 7. ACTION

Status: VERIFIED BY MACHINE CONTRACT / REGRESSION DOCUMENTATION

Canonical form:

```baselscript
ACTION <name>
    ...
END
```

Example:

```baselscript
ACTION save
    message "saved"
END
```

SECTION and ACTION execution/namespace relationships belong to `03_call_execution.md`.

Do not generate a separate invented closing keyword such as:

```text
ENDACTION
```

---

## 8. FORM

Status: VERIFIED BY MACHINE CONTRACT / REGRESSION-BACKED UI DOCUMENTATION

Canonical static form:

```baselscript
FORM <name>
    tile=...
    ...
END
```

Verified example:

```baselscript
FORM main_form
    tile=text text="Name"
    tile=input id=#name
    tile=button text="OK" sec=save
END
```

FORM is normally displayed through:

```baselscript
draw form=<name>
```

Detailed tile grammar belongs to `05_ui.md`.

Do not generate:

```text
ENDFORM
```

---

## 9. LIST

Status: VERIFIED BY MACHINE CONTRACT / REGRESSION-BACKED UI DOCUMENTATION

Canonical static form:

```baselscript
LIST <name>
    tile=...
    ...
END
```

Example:

```baselscript
LIST list1
    tile=title text="Items"
    tile=item id=#name
END
```

A current special declaration is also documented:

```baselscript
LIST view <name>
```

`view` is a modifier; the following token is the LIST name.

Dynamic LIST construction exists, but its lifecycle belongs to `05_ui.md`.

Do not generate:

```text
ENDLIST
```

---

## 10. MENU

Status: VERIFIED BY MACHINE CONTRACT / REGRESSION-BACKED UI DOCUMENTATION

Canonical static form:

```baselscript
MENU <name>
    tile=...
    ...
END
```

Example:

```baselscript
MENU mainmenu
    tile=item text="Settings" scene=setting
    tile=item text="Back" section=back
END
```

Dynamic MENU construction exists and belongs to `05_ui.md`.

Do not generate:

```text
ENDMENU
```

---

## 11. DIALOG

Status: VERIFIED BY MACHINE CONTRACT / REAL SCRIPTS

Canonical form:

```baselscript
DIALOG <name>
    tile=...
    ...
END
```

Example:

```baselscript
DIALOG search_dialog
    tile=title text=#search_dialog_title
    tile=input1 id=#mid_search_query_input text=#mid_search_query_input
    tile=button1 text=#button_search act=apply_search
END
```

Detailed DIALOG tile semantics belong to `05_ui.md`.

Do not generate:

```text
ENDDIALOG
```

---

## 12. POPUPMENU

Status: VERIFIED BY MACHINE CONTRACT / CURRENT RUNTIME

Canonical start:

```baselscript
POPUPMENU <name>
    ...
END POPUPMENU
```

Current runtime code explicitly tracks a POPUPMENU block and closes it when a line starts with `end` and contains `popupmenu`.

Therefore POPUPMENU is different from the common current static FORM/LIST/MENU/DIALOG style that closes with plain `END`.

Do not replace the POPUPMENU terminator with an invented:

```text
ENDPOPUPMENU
```

Use:

```baselscript
END POPUPMENU
```

until a newer verified contract says otherwise.

---

## 13. Generic END rule

Status: VERIFIED BY CURRENT REAL/REGRESSION PATTERNS

For current canonical generated source, these blocks use plain:

```text
END
```

```text
SECTION
ACTION
FORM
LIST
MENU
DIALOG
```

SCENE uses:

```text
END SCENE
```

POPUPMENU uses:

```text
END POPUPMENU
```

Historical source may contain longer forms such as:

```text
END SECTION
END FORM
END SCENE 1
```

Do not normalize historical working code automatically.

For new generated code, prefer the canonical current forms above.

---

## 14. Block names

Status: VERIFIED BY REAL SCRIPTS / MACHINE MODEL

Named blocks are referenced elsewhere by their names.

Examples:

```baselscript
SECTION save
FORM editor
LIST results
MENU mainmenu
DIALOG confirm
```

Literal internal UI targets are normally expected to resolve within the current SCENE.

Detailed reference resolution belongs to `03_call_execution.md`.

Do not invent unnamed static blocks unless a specific block grammar explicitly supports them.

---

## 15. Scene-oriented placement

Status: STRONGLY GROUNDED BY CURRENT CORPUS

Current BaselScript applications place structural blocks inside a SCENE.

Typical pattern:

```baselscript
SCENE=1 title="Example"

SECTION init
    draw form=main_form
END

FORM main_form
    tile=text text="Hello"
    tile=button text="Back" sec=back
END

SECTION back
    call script=mainmenu
END

END SCENE
```

For new code, keep related SECTION/ACTION/UI blocks inside the SCENE that uses them.

Cross-scene execution/reference behavior belongs to `03_call_execution.md`.

---

## 16. Nesting

Status: PARTIALLY VERIFIED

Current structural examples show blocks as scene-level declarations rather than nested structural declarations.

Do not generate a structural block declaration inside another SECTION/ACTION/FORM/LIST/MENU/DIALOG block unless a specific current semantic document and regression test confirms that pattern.

Example to avoid:

```text
SECTION init
    FORM x
        ...
    END
END
```

Prefer sibling declarations inside the SCENE.

---

## 17. Case handling

Status: RUNTIME-GROUNDED / GENERATION POLICY

The runtime normalizes command-leading words in current parser code, and real scripts contain mixed-case historical forms.

For generated reference code, prefer the canonical uppercase structural spelling:

```text
SCENE
SECTION
ACTION
FORM
LIST
MENU
DIALOG
POPUPMENU
END
```

Do not rely on case variation as a language feature in examples.

---

## 18. Do not invent structural syntax

Do not generate:

```text
BEGIN SECTION
ENDSECTION
ENDFORM
ENDLIST
ENDMENU
ENDDIALOG
ENDSCENE
class
function
procedure
widget
screen
page
```

unless a future verified BaselScript contract explicitly introduces such constructs.

---

## 19. Machine-contract interpretation

`blocks.def` contains rows such as:

```text
SECTION;sec;Prefix;sec;Prefix
ACTION;act;Prefix;act;Prefix
MENU;menu;Prefix;menu;Exact
LIST;list;Prefix;list;Exact
DIALOG;dia;Prefix;dia;Prefix
FORM;form;Prefix;form;Exact
POPUPMENU;popupmenu;Prefix;popupmenu;Exact
```

These rows describe StructureValidator matching behavior.

They are not a complete literal source grammar.

In particular, internal matching tokens such as `sec`, `act`, and `dia` must not be copied directly into generated block declarations.

`scene.def` is authoritative for SCENE's special structural metadata.

---

## 20. Scope limits

This file intentionally does not define:

- complete SCENE parameter semantics;
- CALL target resolution;
- SECTION/ACTION callable namespace behavior;
- UI tile parameters;
- dynamic FORM/LIST/MENU construction;
- event routing parameters;
- cross-script execution;
- platform-specific UI behavior.

Those belong to their routed semantic categories.
