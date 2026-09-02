BaselScript regression batch 04
================================

Purpose:
- confirm real FILE + DIRECTORY_LIST + READ + static LIST flow
- confirm both external-call spellings:
    call scr=...
    call script=...
- confirm responsibility boundary:
    ReferenceValidator intentionally does NOT prove external script existence.
    Missing external scripts are therefore runtime errors, not validator errors.

Sources:
R11 <- 000_template_list_from_directory.script, unchanged body
R12 <- R11, only first external scr target changed
R13 <- _colors.script, unchanged body
R14 <- R13, only external script target changed

Run:
R11:
  - list must display
  - BACK or SELECT should reach 000_templates

R12:
  - MUST pass Validator
  - trigger the changed BACK/SELECT path
  - expect runtime missing-script error

R13:
  - colors list must display
  - BACK should reach _setting_colorscheme

R14:
  - MUST pass Validator
  - press BACK
  - expect runtime missing-script error

Expected result notation:
R11 = PASS
R12 = PASS - VALIDATOR OK, RUNTIME MISSING SCRIPT
R13 = PASS
R14 = PASS - VALIDATOR OK, RUNTIME MISSING SCRIPT

No interpreter changes during this batch.
