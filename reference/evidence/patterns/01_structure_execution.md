# Structure and execution patterns

Status: STRONGLY CORPUS-GROUNDED / CURRENT CONTRACT CHECKED

## Core execution model

A normal BaselScript application is organized as:

```text
SCENE
    -> SECTION/ACTION entry or event logic
    -> FORM/LIST/MENU/DIALOG declarations
    -> CALL or DRAW to activate the required UI/scene/script
```

The corpus contains 425 scripts. `SECTION` appears across almost the entire corpus,
and `call` is one of the dominant composition commands.

Observed call families include:

```text
call scene=...
call script=...
call scr=...
call menu=...
call section=...
call sec=...
call list=...
call dialog=...
```

Use the canonical form documented by `03_call_execution.md`. Do not derive a new
CALL target family from a name.

## Minimal scene lifecycle

```baselscript
SCENE=1 title="Example"

SECTION init
    message "Hello"
END

END SCENE
```

`SECTION init` is the normal place for scene-start orchestration in current scripts.

## Scene to menu

```baselscript
SCENE=1 title="Main"

SECTION init
    call menu=mainmenu
END

MENU mainmenu
    tile=item text="Open" sec=open_item
    tile=item text="Back" script=empty
END

SECTION open_item
    message "Open"
END

END SCENE
```

The MENU declaration and the SECTION it targets are sibling blocks in the same SCENE.

## Scene to another scene

```baselscript
SCENE=1

SECTION init
    call scene=second
END

END SCENE

SCENE=second

SECTION init
    message "Second scene"
END

END SCENE
```

Do not place a SECTION/ACTION declaration inside another structural block.

## Script transition

Canonical current forms are documented by `03_call_execution.md`.

```baselscript
call script=other_script
```

A scene can be selected when the routed semantic file confirms the form:

```baselscript
call script=other_script scene=2
```

Do not invent function-call syntax such as `openScript(...)`.

## Same-scene callable rule

SECTION and ACTION belong to the current callable namespace. New generated code should
keep the called block in the scene where it is used unless the task intentionally
changes scenes or scripts.

## Generation rule for complete scripts

When the user asks for a complete runnable example, include the required orchestration,
not only the target declaration.

For example, a FORM example normally needs:

```text
SCENE
SECTION init -> draw form=...
FORM ...
event SECTION/ACTION ...
END SCENE
```

A LIST example normally needs:

```text
data preparation if required
SCENE
SECTION init -> call list=...
LIST ...
event SECTION/ACTION ...
END SCENE
```

The detailed lifecycle is defined in the routed domain pattern.
