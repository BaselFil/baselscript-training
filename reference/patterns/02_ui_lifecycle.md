# UI lifecycle patterns

Status: CURRENT SEMANTICS + REPEATED CORPUS + RUNTIME/UI TESTS

This pattern file composes UI lifecycles. It does not replace the domain
semantic files.

For FORM layout and geometry, `reference/semantics/19_form.md` is authoritative.

## Activation matrix

```text
FORM   -> draw form=<name>
LIST   -> call list=<name>
MENU   -> call menu=<name>
DIALOG -> call dialog=<name>
```

Do not interchange these activation forms.

## FORM lifecycle

For a complete FORM script:

```text
SCENE
-> SECTION init
-> optional orientation/file/data setup
-> draw form=<name>
-> FORM <name>
-> target SECTION/ACTION blocks
-> END SCENE
```

If structured file data is used, the file declaration belongs inside the
initializing SECTION before `read`.

Example:

```baselscript
SCENE=1 title="Person"

SECTION init

    file name=person \
        record=(#first_name,#last_name,#street,#city,#birthday) \
        dir=#_directory_files_examples

    read file=person dir=#_directory_files_examples

    draw form=person_form

END

FORM person_form
    ...
END

END SCENE 1
```

## Landscape FORM rule

A landscape FORM must use the nominal BaselScript landscape geometry, not the
pixel width of a resized desktop window.

Canonical nominal width:

```text
1280
```

For an ordinary two-column landscape form, prefer the tested baseline from
`19_form.md`:

```text
left:  x=60  w=550
right: x=670 w=550
```

Field labels above inputs:

```baselscript
tile=text x=60 y=80 w=550 text="First name"
tile=input id=#first_name x=60 y=135 w=550 h=65

tile=text x=670 y=80 w=550 text="Last name"
tile=input id=#last_name x=670 y=135 w=550 h=65
```

Do not compress a requested landscape two-column form to narrow columns such as
`w=280` merely because the current desktop window looks narrow.

Do not put the label beside the input when the requested layout is "field name
above input".

## Forced landscape lifecycle

When the user explicitly requests landscape-only behavior and the current script
name is known:

```baselscript
SECTION init
    if #_orientation == portrait
        set orient=landscape
        call scr=<current_script>
    endif

    ...
    draw form=<landscape_form>
END
```

For an adaptive UI, do not force orientation. Choose the relevant portrait or
landscape form from the current orientation.

## FORM semantic precedence

When generating a FORM:

1. load `19_form.md`;
2. apply its verified geometry and label/input rules;
3. use this pattern only to compose the lifecycle;
4. never let a generic corpus example override the verified `19_form.md`
   layout rules.

## LIST lifecycle

For a file-backed LIST, also load `06_files_data.md`.

```baselscript
SCENE=1 title="People"

SECTION init

    file name=example_person \
        record=(#first_name,#last_name,#city,#street,#birthday) \
        dir=#_directory_files_examples

    read file=example_person dir=#_directory_files_examples

    call list=person_list

END

LIST person_list
    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280
    tile=item row=1 col=2 text=#last_name w=280

    tile=item row=2 col=1 text=#street w=600

    tile=item row=3 col=1 text=#city w=300
    tile=item row=3 col=2 text=#birthday w=300

    tile=select sec=selected_person
END

END SCENE 1
```

LIST rows may use different numbers of columns. Use `row` / `col` when fields
should remain visually independent and require separate width/style/alignment control.
Only fields intended to appear next to each other should share the same `row`.

If several source fields should become one textual value instead, concatenate them
in an additional SECTION into a working field and display that working field as one
LIST item. Do not invent inline concatenation syntax inside `tile=item`.

Exact LIST layout semantics come from `reference/semantics/21_list.md`.

## MENU lifecycle

```baselscript
SECTION init
    call menu=mainmenu
END

MENU mainmenu
    ...
END
```

## DIALOG lifecycle

```baselscript
SECTION init
    call dialog=login
END

DIALOG login
    ...
END
```

Exact DIALOG field semantics come from `20_dialog.md`.

## Generation rule

For complete scripts, generate the complete lifecycle. Do not return only a
FORM/LIST declaration when initialization, data preparation or activation is
required.
