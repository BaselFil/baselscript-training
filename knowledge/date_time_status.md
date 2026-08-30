# BaselScript Date and Time Status

## Purpose

This file documents CURRENT date/time semantics that are not visible from function name and arity alone.

AI rule:

A missing one-step convenience function is not proof that BaselScript cannot perform the task.
Check side effects, system variables, and documented compositions before refusing.

## Current weekday name

For the current date, BaselScript already provides a localized weekday name.

Confirmed pattern:

```baselscript
#current_date=$date()
#weekday_name=#_current_weekday_name
```

The current runtime sets these fields when current-date functions initialize the date state:

```text
#_current_date
#_current_year
#_current_month
#_current_0month
#_current_month_name
#_current_day
#_current_0day
#_current_weekday_name
#_current_weekday
#_current_hour
#_current_minute
#_current_secund
#_current_msecond
#_left_days_year
#_day_of_year
```

`#_current_weekday_name` is localized through the configured BaselScript language.

Example documented by the BaselScript reference:

```text
#config_language = GER
#_current_weekday_name = Samstag
#_current_weekday = 6
```

The same current-date field population is documented for:

```baselscript
$date()
$date.current_long()
$date.current_short()
```

Therefore, for a question such as "show the current weekday name", do not answer that BaselScript
has no weekday-name support.

Use:

```baselscript
#d=$date()
message #_current_weekday_name
```

## Day-of-week number for an arbitrary date

CURRENT function definitions include:

```text
day_of_week
date.day_of_week
```

with 1 or 2 parameters.

Examples:

```baselscript
#weekday=$day_of_week(20240928,yyyyMMdd)

#date="2024-09-28"
#weekday=$date.day_of_week(#date)
```

Important semantic detail:

`$day_of_week(...)` returns the runtime `DayOfWeek` number:

```text
0 = Sunday
1 = Monday
2 = Tuesday
3 = Wednesday
4 = Thursday
5 = Friday
6 = Saturday
```

This is different from `#_current_weekday`, where the current-date initializer converts Sunday from
`0` to `7`.

So:

```text
$day_of_week(...)     -> 0..6, Sunday = 0
#_current_weekday     -> 1..7, Sunday = 7
```

Do not merge these two conventions.

## Localized weekday name for an arbitrary date

The current runtime obtains localized weekday names through the BaselScript dictionary.

For current-date state it performs the equivalent of:

```text
weekday = DayOfWeek
if weekday == 0 -> weekday = 7
GetMessage(Config.Language, weekday)
```

Therefore an arbitrary date can be composed from CURRENT primitives:

```baselscript
#weekday=$day_of_week(#date)

if #weekday == 0
    #weekday=7
endif

#weekday_name=$get_message(#config_language,#weekday)
```

This composition is derived from current runtime behavior and the documented dictionary function.

If maintaining an application that already has its own weekday dictionary keys or mapping table,
preserve that working application convention.

## Date formatting

Do not invent a foreign-language formatting convention such as a .NET `dddd` token unless that exact
BaselScript behavior is documented and confirmed.

For weekday names, prefer the confirmed BaselScript mechanisms above.

## AI decision rule

For date/time requests:

1. read this file
2. read `language/functions.def`
3. distinguish current-date fields from arbitrary-date calculations
4. check function side effects and system variables
5. use a documented composition when no dedicated convenience function exists
6. only say "unsupported" if neither a direct primitive nor a confirmed composition exists
