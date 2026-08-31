# DIALOG semantics

## Input hint and password field

Status: VERIFIED BY RUNTIME / UI TEST

A dialog can combine labels, input fields, hints and password input.

Canonical example:

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

Rules:

- `hint` displays helper text inside an input field before the user enters a value.
- `control=password` hides the entered password characters.
- `tile=textN` can be used as a label for the corresponding `tile=inputN`.
- `tile=buttonN` can call a section using `section=...`.
- Keep the numbering of related dialog elements clear and consistent.
