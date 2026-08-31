# DIALOG input semantics

Status: VERIFIED BY RUNTIME / UI TEST

This file extends `reference/semantics/05_ui.md` with a verified DIALOG input pattern.

## Hint and password input

Canonical verified example:

```baselscript
SCENE=1 title="Login"

SECTION init
    call dialog=login
END

DIALOG login

    tile=title text="Anmeldung"

    tile=text1 text="Benutzername"
    tile=input1 name=#user hint="Name eingeben"

    tile=text2 text="Passwort"
    tile=input2 name=#password hint="Passwort eingeben" control=password

    tile=button1 text="OK" section=login_ok
    tile=button2 text="Abbrechen" section=back

END

SECTION login_ok
    message $concat("Benutzer: ",#user)
END

SECTION back
    call script=empty
END

END SCENE 1
```

Verified behavior:

- `hint` displays helper text inside the input before a value is entered.
- `control=password` hides the entered password characters.
- `tile=textN` can act as the label for the corresponding `tile=inputN`.
- `tile=buttonN` can route to a section with `section=...`.

## AI generation rules

- Use `hint` for helper text inside DIALOG inputs when requested.
- Use `control=password` for password entry.
- Keep related numbered elements clear and consistent.
- Do not invent a generic dialog API from another UI toolkit.
