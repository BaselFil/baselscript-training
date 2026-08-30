# BaselScript Network, Media, and Device Status

## Purpose

This file documents CURRENT or strongly grounded BaselScript network, media, and platform-facing
features for AI generation and review.

This area is platform-sensitive. A command name can be formally present while its exact parameter
contract or behavior differs between Windows and Android. Do not invent parameters where only the
action name is known.

## Evidence classes

Use these classes in this file:

- CURRENT-CONFIRMED - present in current definitions and supported by runtime/corpus evidence.
- CURRENT-WEAK-CONTRACT - present in current definitions, but canonical parameters are not yet fully documented.
- CORPUS/RUNTIME-CANDIDATE - seen in real scripts or runtime but missing from the current formal definitions; do not generate by default until synchronized.

## DOWNLOAD

`download` is formally CURRENT and heavily used in the corpus.

Observed patterns include:

```baselscript
download url=#url string=#string sec=download_done
```

```baselscript
download file=#name_file url=#link directory=#DIR
```

```baselscript
download url=#url stream=csv file=temp
```

```baselscript
download script=#script from_directory=script to_directory=APPDIR/script
```

Observed parameter families include:

```text
url
string
file
directory / dir
stream
script
from_directory
to_directory
sec / section_download_done
```

Do not assume all parameter combinations are valid for all download modes. Preserve the mode used by
a matching real script.

## UPLOAD

`upload` is formally CURRENT and repeatedly used.

Observed patterns:

```baselscript
upload image=#image_name dir=#directory upload_dir="screens" url=#url
upload script=#file_name
upload typ=examples script=#file
upload typ=my script=#file
```

The upload action is mode-dependent. Do not invent a universal REST/HTTP request grammar around it.

## SITE / GOOGLE integrations

Real CALL integrations include:

```baselscript
call site=#link
call google=navigation address=#address
call google=search lat=#lat lng=#lng request=#request
call google=earth file=#file_name dir=APPDIR/files_kml
```

These are runtime/platform integrations. Do not treat `google=` as a normal script block reference.
Do not invent new map subcommands without runtime evidence.

## RUN

`run` is formally CURRENT and overloaded in real scripts.

One family evaluates/transforms content:

```baselscript
run from=#function to=#result
```

Another Windows-oriented family starts an external program:

```baselscript
run program=#global_chrom parameter=#link
```

Because these are semantically different modes, AI must preserve an existing working pattern and
must not infer that all `run` parameters are interchangeable.

External-program launch is platform-specific and should not be generated for Android without
confirmed Android behavior.

## SOUND

`sound` is formally CURRENT and runtime/corpus-confirmed.

Observed start pattern:

```baselscript
sound mode=start file=#sound dir=#_directory_my_sound
```

A direct path form also occurs:

```baselscript
sound mode=start file=APPDIR/MYDATA/sound/#sound
```

Do not invent playback modes beyond those confirmed by existing scripts/runtime.

## MUSIC

`music` is formally CURRENT and runtime/corpus-confirmed.

Observed controls include:

```baselscript
music mode=start file=#music dir=#_directory_music_examples
music mode=pause
music mode=resume
music mode=stop
music volume=#volume
music position=#position
```

Treat `volume=` and `position=` as state controls of the current music playback context.

## NOTIFICATION

The action definitions contain the prefix family:

```text
notif;Prefix;...
```

Runtime and corpus use the full `notification` spelling, which is accepted by the `notif` prefix
family.

Observed definition pattern:

```baselscript
notification number=1 title="mainmenu" text="press to jump" script=_mainmenu vib=1 sound=off
```

Observed activation pattern:

```baselscript
start notification=1
```

Notification behavior is platform-sensitive. Preserve the parameters of known working examples.

## THUMBNAIL

The current definitions contain `thumb` and `create_thumb` prefix families. Runtime accepts forms
beginning with these prefixes, and real scripts use `thumbnail`.

Observed forms:

```baselscript
thumbnail i_dir=APPDIR/files/images/examples i_f=2.png o_dir=#_directory_thumbnails o_f=22_thumbnail.png w=65 h=65
```

```baselscript
thumbnail i_d=#_directory_images_examples i_f=sample.jpg o_d=#_directory_thumbnails o_f=sample1.png scale=10%
```

Observed parameter aliases include long/short directory forms and width/height vs scale.

## CLIPBOARD_SET

`CLIPBOARD_SET` is formally CURRENT and used by MID.

Canonical project evidence:

```baselscript
CLIPBOARD_SET value=#edit_password
```

MID checks runtime result/error variables after the action. When maintaining that code, preserve the
existing error-handling contract rather than assuming clipboard success.

Clipboard support is platform-dependent.

## VOICE / SPEAK

Real runtime/corpus integrations include:

```baselscript
call voice=#text sec=voice
call speak=#text
```

These are CALL integrations rather than standalone actions in the formal action list. Generate only
when the target platform is known to support them.

## START / STOP and timers

`start` and `stop` are formally CURRENT. Real scripts use them for timers and notification start.

Timer examples:

```baselscript
start timer=1 interval=200 sec=timer1
start timer=1 interval=1000 act=timer1
```

Do not assume timer syntax is identical to unrelated START/STOP modes.

## Platform-specific action decisions

Current training decisions:

- `driver_list` is CURRENT because the current dispatcher calls `Actions.DriverList(content)`,
  but its exact parameter/result contract remains weakly documented.
- standalone `vibrate` is not CURRENT. Windows recognizes an old no-op handler, while Android
  vibration is confirmed only inside the notification subsystem.
- standalone `open`, `mail`, and `pdf` are not part of the current action contract.

For Android notifications, vibration can be enabled by the notification data itself. Do not turn
that behavior into a standalone `vibrate` action.

## Corpus/runtime candidates not synchronized with definitions

Some old scripts contain platform actions such as `installed_list`. It is not present in the
current formal `actions.def` used by this training update.

Therefore treat such names as CORPUS/RUNTIME-CANDIDATE, not CURRENT generation syntax, until the
maintainer confirms and synchronizes them.

## Platform rules

1. Windows behavior is not automatically Android behavior.
2. Android file/network operations must respect the current scoped/document access model.
3. External program launch is inherently platform-specific.
4. Media, notification, vibration, speech, and clipboard behavior may require platform adapters.
5. Do not turn historical Android storage paths into recommended new code.

## AI generation rules

- Prefer commands with both formal definition and real evidence.
- Preserve known working parameter sets.
- Do not invent generic HTTP, media, notification, device, or OS APIs.
- Separate runtime integrations from internal script references.
- Flag platform assumptions explicitly during review.
- If a command is definition-only with no parameter contract, name it but do not fabricate syntax.
