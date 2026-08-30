# BaselScript UI Status

## Purpose

This file teaches AI systems how the CURRENT BaselScript UI model is used in real scripts.
It complements `blocks.def`, `scene.def`, `baselscript-language.json`, regression evidence,
and the real script corpus.

It does not create new syntax. When this file conflicts with a language definition or
confirmed regression result, the higher-authority source wins.

## Evidence status

Confirmed or strongly grounded CURRENT UI families:

- `FORM`
- `LIST`
- `MENU`
- `DIALOG`
- `POPUPMENU` as a defined structural block
- static UI declarations with `tile=...`
- runtime/dynamic UI construction with `clear ...` and `create tile=...`
- `draw form=<name>`
- `call list=<name>`
- `call menu=<name>`
- `LIST view <name>`

Regression-confirmed examples include real FORM rendering, static LIST, dynamic LIST with
`tile=file`, and dynamic MENU construction.

## Structural model

BaselScript UI is scene-oriented. A typical screen contains a `SCENE`, an initialization
SECTION, and one or more UI blocks.

```baselscript
SCENE=1 title="Example"

SECTION init
    draw form=main_form
END

FORM main_form
    tile=text text="Name"
    tile=input id=#name
    tile=button text="OK" sec=save
END

SECTION save
    trace #name
END

END SCENE
```

Use the exact block names and END behavior defined by `blocks.def` and `scene.def`.
Do not invent `ENDFORM`, `ENDLIST`, `ENDMENU`, or similar closing keywords.

## FORM

A FORM is a static layout block selected with `draw form=<name>`.

Regression-confirmed pattern:

```baselscript
SECTION init
    #form=0
    draw form=#form
END

FORM 0
    tile=text text="Address"
    tile=input id=#city
    tile=button text="Back" sec=back
END
```

Real corpus FORM tiles commonly include:

- `text`
- `input`
- `button`
- `property` / `prop`
- `image`
- `checkbox`
- `radiobutton`
- `seekbar`
- `spinner`
- `switch`
- `togglebutton`

The corpus contains historical abbreviations and numbered tile forms. Do not generalize a
rare spelling into a new tile type. For uncommon tiles, inspect a matching real example
and the current UI contract before generating.

## LIST

Static LIST example:

```baselscript
SECTION init
    read file=example_person dir=APPDIR/files/examples
    call list=list1
END

LIST list1
    tile=file id=example_person
    tile=title text="List of people"
    tile=item id=#first_name
    tile=item id=#last_name
    tile=button text=< act=back
END
```

CURRENT special declaration:

```baselscript
LIST view <name>
```

`view` is a modifier. The following token is the LIST name.
Do not interpret `view` itself as the list name.

## Dynamic LIST

Dynamic LIST construction is confirmed.

```baselscript
SECTION init
    directory_list dir=APPDIR/script target_file=dir_list
    read file=dir_list sort="order by (#file_name)"

    clear list_view=ccc
    create tile=file id=dir_list dir=APPDIR/MYDATA/temp
    create tile=title text="FILES"
    create tile=item1 id=#file_name

    call list=ccc
END
```

Important:

- `tile=file` may be created dynamically.
- A dynamic UI name is not required to have a static block declaration when the runtime
  constructs it.
- Do not reject a dynamic LIST solely because no static `LIST <name>` exists.

## MENU

Static MENU:

```baselscript
MENU mainmenu
    tile=item text="Settings" scene=setting
    tile=item text="Back" section=back
END
```

Dynamic MENU is CURRENT and has a dedicated regression example. The canonical lifecycle is:

```text
clear menu=<name>
create tile=...
create tile=...
call menu=<name>
```

Confirmed example:

```baselscript
SECTION e1
    clear menu= m2
    create tile=title text= "menu 2" c=blue sty=italic
    create tile=item1 text= "option 111" sec=e11
    create tile=item2 text= "option 222" sec=e22
    create tile=item3 text= "call scene2" sce=2
    create tile=item4 text= back sec=back_to_m1
    call menu=m2
END
```

This confirms all of the following:

- `clear menu=m2` establishes a runtime-created MENU name;
- subsequent `create tile=...` lines populate that current MENU;
- `sec=` routes a menu tile to SECTION/ACTION callable targets;
- `sce=` routes a menu tile to a SCENE;
- `call menu=m2` displays the dynamically created MENU;
- no static `MENU m2 ... END` declaration is required for this pattern.

Validator note: the maintainer's current local build was observed to reject the confirmed
`R03_DYNAMIC_MENU_REAL_OK.script` with `MENU 'm2' does not exist in SCENE '1'`. The inspected
`latest_rev19/ReferenceValidator.cs` already contains symbol-building logic for `clear menu=...`
and `create menu=...`. Therefore this local failure is a validator/build synchronization issue,
not evidence that dynamic MENU syntax is invalid. Keep R03 as a required regression check after
the validator is synchronized.

Do not assume numbered `item1`, `item2`, etc. are universally interchangeable with every
UI block. Use them when supported by the matching real pattern.

## DIALOG

`DIALOG` is a CURRENT structural block in `blocks.def` and has real corpus/regression
coverage. Follow existing DIALOG examples for tile composition and invocation.

Do not invent a generic dialog API from another language or UI toolkit.

## Event targets

Real UI tiles route execution using parameters such as:

- `sec=` / `section=`
- `act=` / `action=`
- `scene=` / `sce=`
- `script=` / `scr=`

These spellings are corpus evidence, not permission to invent further aliases.
When editing an existing script, preserve its working style.

SECTION and ACTION targets share the callable namespace according to current validation
semantics. Static references should resolve when statically knowable; dynamic targets may
only be resolvable at runtime.

## Common UI parameters

Frequently observed parameters include:

```text
id text x y w h c bg s sty style g gravity sec act scene script dir visible
```

This list is descriptive, not a universal parameter contract. Not every tile accepts every
parameter. For a specific tile, prefer the current UI contract and repeated real examples.

## Generation rules

- Prefer a simple static FORM/LIST/MENU when runtime construction is unnecessary.
- Use dynamic `clear` + `create tile=...` only when the screen is genuinely built at runtime.
- Preserve exact known target spellings; do not invent event parameters.
- Do not infer that every corpus tile spelling is CURRENT.
- Do not generate rare tile types without inspecting a real example.
- Keep UI logic in SECTION/ACTION blocks rather than inventing inline language constructs.

## Evidence examples

High-value real/regression sources:

```text
checkpoint/latest_rev19/tests/FORM/R06_FORM_REAL_OK.script
checkpoint/latest_rev19/tests/LIST/R01_STATIC_LIST_REAL_OK.script
checkpoint/latest_rev19/tests/LIST/L06_DYNAMIC_LIST_FILE_TILE_OK.script
checkpoint/latest_rev19/tests/MENU/R03_DYNAMIC_MENU_REAL_OK.script
corpus/000_template_dialog_message.script
corpus/creating_dynamic_menu.script
corpus/examples_list.script
corpus/EXPENSA_analysis.script
```
