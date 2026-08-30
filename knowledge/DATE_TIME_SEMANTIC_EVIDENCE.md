# Date/time semantic evidence

Status: current runtime/reference evidence.

## Runtime behavior

Current `Functions.cs` date initialization sets:

```text
#_current_month_name
#_current_weekday_name
#_current_weekday
#_left_days_year
#_day_of_year
```

For weekday state:

```text
DayOfWeek Sunday 0 is converted to 7 for #_current_weekday.
The localized name is loaded from the BaselScript dictionary.
```

## Arbitrary date

`day_of_week` / `date.day_of_week` returns the runtime DayOfWeek numeric value directly:

```text
0 Sunday
1 Monday
...
6 Saturday
```

This differs from the 1..7 current-state variable convention.

## Reference example

The BaselScript reference documents:

```text
#_current_weekday_name = Samstag
#_current_weekday = 6

$day_of_week(20240928,yyyyMMdd) = 6
$date.day_of_week("2024-09-28") = 6
```

These semantics are summarized in `knowledge/date_time_status.md`.
