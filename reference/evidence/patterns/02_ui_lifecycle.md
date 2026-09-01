# UI lifecycle patterns

Status: CURRENT SEMANTICS + REPEATED CORPUS + RUNTIME/UI TESTS

Static UI dominates the current corpus:

```text
FORM    245 declarations / 159 files
MENU    187 declarations / 155 files
LIST    141 declarations / 121 files
DIALOG   91 declarations / 66 files
tile   4852 occurrences / 340 files
```

For ordinary application generation, prefer the static lifecycle unless the user
explicitly needs dynamic UI.

## Activation matrix

```text
FORM   -> draw form=<name>
LIST   -> call list=<name>
MENU   -> call menu=<name>
DIALOG -> call dialog=<name>
```

Do not interchange these activation forms.

## FORM lifecycle

```baselscript
SCENE=1 title="Person"

SECTION init
    draw form=person_form
END

FORM person_form
    tile=text x=50 y=60 w=700 text="Name"
    tile=input name=#name x=50 y=115 w=700 h=65
    tile=button x=450 y=220 w=300 h=70 text="Save" sec=save
END

SECTION save
    message #name
END

END SCENE
```

For one-line `tile=text` labels, omit an arbitrary explicit height. Current runtime/UI
testing showed that forcing a small height can disturb vertical spacing. Use explicit
height for multiline text or an intentionally fixed text area.

## MENU lifecycle

```baselscript
SCENE=1 title="Menu"

SECTION init
    call menu=mainmenu
END

MENU mainmenu
    tile=item text="New" sec=start_new
    tile=item text="Back" script=empty
END

SECTION start_new
    message "New"
END

END SCENE
```

MENU items can route to the target type documented by current UI semantics. Do not
invent a generic event-handler API.

## DIALOG lifecycle

```baselscript
SCENE=1 title="Login"

SECTION init
    call dialog=login
END

DIALOG login
    tile=title text="Login"

    tile=text1 text="Username"
    tile=input1 name=#user hint="Enter username"

    tile=text2 text="Password"
    tile=input2 name=#password hint="Enter password" control=password

    tile=button1 text="OK" section=login_ok
    tile=button2 text="Cancel" section=back
END

SECTION login_ok
    message #user
END

SECTION back
    call script=empty
END

END SCENE
```

## File-backed LIST lifecycle

A LIST that displays structured file records is a cross-domain pattern. It requires
the `files_data` route as well as the LIST/UI route.

```baselscript
file name=example_person \
    record=(#first_name,#last_name,#city,#street,#birthday) \
    dir=#_directory_files_examples

SCENE=1 title="People"

SECTION init
    read file=example_person dir=#_directory_files_examples
    call list=person_list
END

LIST person_list
    tile=file name=example_person

    tile=item row=1 col=1 text=#first_name w=280 st=bold
    tile=item row=1 col=2 text=#last_name w=280 st=bold

    tile=item row=2 col=1 text=#city w=280
    tile=item row=2 col=2 text=#birthday w=280

    tile=item row=3 col=1 text=#street w=600

    tile=select sec=selected_person
END

SECTION selected_person
    message $concat(#first_name," ",#last_name)
END

END SCENE
```

Within one source record:

```text
row -> vertical row
col -> horizontal field within the row
w   -> explicit item width
```

The same layout is repeated for every source record. If columns must align across rows,
use matching explicit widths.

## Orientation/layout responsibility

Use SCENE for a logical page or application step. Multiple FORMs in one SCENE can
represent alternative visual layouts, for example portrait and landscape. Do not
create extra scenes solely because the same logical page needs another orientation.

Detailed FORM layout rules remain in `19_form.md`.

## Dynamic UI

Dynamic `create`, `clear` and `set` forms are present in the corpus, but they are much
less common than static UI and have more validator/runtime edge cases.

Generation rule:

- prefer static FORM/LIST/MENU/DIALOG for ordinary examples;
- use dynamic UI only when the task requires runtime construction;
- load the exact current UI semantics before generating dynamic syntax.
