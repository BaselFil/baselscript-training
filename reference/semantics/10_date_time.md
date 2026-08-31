# 10 - Date / time

Status: STRONGLY VERIFIED FOR CURRENT-DATE STATE AND WEEKDAY SEMANTICS

Primary source:

- `language/functions.def`

## Current date initialization

Verified:

```baselscript
#current_date=$date()
```

Current-date initialization populates runtime fields including:

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

For the current weekday name:

```baselscript
#d=$date()
message #_current_weekday_name
```

Do not invent `weekday_name()`.

## Arbitrary-date weekday number

Current function:

```text
day_of_week / date.day_of_week
```

Examples:

```baselscript
#weekday=$day_of_week(20240928,yyyyMMdd)

#date="2024-09-28"
#weekday=$date.day_of_week(#date)
```

Important distinction:

```text
$day_of_week(...) -> 0..6, Sunday = 0
#_current_weekday -> 1..7, Sunday = 7
```

Do not merge these conventions.

## Localized weekday name for arbitrary date

Confirmed composition:

```baselscript
#weekday=$day_of_week(#date)

if #weekday == 0
    #weekday=7
endif

#weekday_name=$get_message(#config_language,#weekday)
```

## Other CURRENT date/time families

```text
date.current_date_long
date.current_date_short
date.current_time
date.current_ticks
date.days_in_month
date.date_format
date.strings_to_dateformat
date.add_days
add_hours
add_minutes
add_seconds
add_months
add_years
datetime
diff_days
diff_months
diff_dates
day_of_year
week_of_date
day_of_date
month_of_date
year_of_date
date.compare
date.in_interval
```

Do not import .NET/Python/Java date-format tokens unless specifically verified for BaselScript.
