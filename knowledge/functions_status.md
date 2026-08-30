# BaselScript Functions Status

## Purpose

This file teaches AI systems how to use BaselScript functions without inventing names,
aliases, or parameter counts.

`functions.def` and `baselscript-language.json` are the function contract. The real corpus
provides examples and prevalence evidence.

## Function call model

Functions are called with `$`:

```baselscript
#text=$concate("A",#value)
#len=$length(#text)
#date=$date("YYYY-MM-dd")
```

Function names are case-sensitive according to the current function registry.
Do not change case casually.

## Canonical names and aliases

Each line of `functions.def` has the form:

```text
canonical;aliases;arity;requirement
```

An alias is CURRENT only when it is explicitly listed in `functions.def`.
Corpus similarity is not enough.

Example family:

```text
string.concate
```

with defined aliases including commonly used corpus spellings such as:

```text
concate
concat
conc
con
```

When creating new code, prefer a clear canonical or well-established defined alias.
When editing an existing script, preserve its valid working spelling unless normalization
is part of the task.

## Arity

Respect the parameter rule from `functions.def`.
Examples:

```text
pow             -> exactly 2
math.random     -> 2 or 4
substr          -> 2 to 3
string.repeat   -> exactly 2
```

`*` means no migrated static parameter-count rule exists yet. It does not mean that every
possible argument list is semantically useful.

## Runtime requirements

Some functions have runtime requirements, for example an application database or canvas.
Do not call them as if they were context-free utilities.

## Major CURRENT function families

### String

Frequently used and defined families include:

```text
string.concate / concate / concat
cont / contains
substr
lower
upper
string.replace / replace
string.length / length
starts_with
ends_with
string.trim
string.format
string.money
```

Example:

```baselscript
#title=$concat("KEY ",#c1," ",#c2)
if $contains(#scriptname,".script")==0
    #scriptname=$add_extention(#scriptname,".script")
endif
```

### Math

Defined families include:

```text
math.abs
math.min
math.max
math.round
math.truncate
math.random
math.sin
math.cos
math.sqrt
pow
```

Use only defined spellings. Similar English words are not automatic aliases.

### Arrays

Defined function families include:

```text
array.counter / array.length and defined aliases
array.min
array.max
array.sum
array.contains
array.add
array.get_first_index
array.count_of_string
array.count_of_value
array.clear
```

Regression confirms indexed arrays, FOR interaction, and `$array.sum` behavior.

Example:

```baselscript
#max=$array.max(#values)
#sum=$array.sum(#values)
```

### Hash arrays

Defined helpers include:

```text
hash_array.get_value
hash_array.length
```

Regression confirms real hash-array clear/set/get-value behavior.

### Date/time

Defined families include:

```text
date
date.current_date_long
date.current_date_short
date.current_time
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
```

Example:

```baselscript
#edit_datetime=$date("YYYY-MM-dd HH:mm:ss")
#edit_date=$substr(#edit_datetime,0,10)
```

### File/directory functions

See `files_status.md`. Only names defined in `functions.def` are CURRENT.

### UI/form functions

Defined helpers include functions for form text, coordinates, touch position, and text
exchange. Use them only with the current documented/defined parameter count.

### Draw/canvas functions

See `graphics_status.md`. Several `draw.*` functions require a canvas context.

## High-frequency corpus spellings

The corpus strongly demonstrates the following defined spellings:

```text
concate
concat
get_message
form.exchange_text
contains
form.text
math.random
math.truncate
substr
get_default_value
math.round
array.max
date
datetime
upper
string.replace
length
lower
```

High frequency is useful style evidence, but it does not override the definition file.

## Corpus-only function spellings requiring review

The audit found the following function spellings in real scripts that are absent from the
current `functions.def`:

```text
create_directory
copy_directory
delete_directory
rename_directory
as_it_is
date.format
draw.tile_color
exist_in_array
math.cosinus
math.sinus
string.rgb
string.rgb_hex
string.upper_substring
txt
upper_substring
```

Classification:

```text
CORPUS-ONLY / UNVERIFIED
```

Rules:

- Do not generate these as CURRENT syntax.
- Do not add them as aliases based only on similarity.
- First verify runtime/parser support and decide whether each is CURRENT alias, LEGACY, or ERROR.
- A spelling used once is especially weak evidence.

Likely semantic similarity is not proof. For example, `math.cosinus` resembles the defined
`math.cos`, but AI must not silently replace or bless it without explicit review.

## Function-generation procedure

Before using an unfamiliar function:

1. Find it in `functions.def` or `baselscript-language.json`.
2. Check its exact case.
3. Check whether the chosen spelling is canonical or an explicitly defined alias.
4. Check arity.
5. Check runtime requirement.
6. Inspect a real corpus example if usage remains unclear.
7. If absent from the definition, classify it as unverified rather than guessing.

## Evidence examples

```text
checkpoint/latest_rev19/tests/ARRAY_FUNCTION/R15_SIMPLE_ARRAY_REAL_OK.script
checkpoint/latest_rev19/tests/ARRAY_FUNCTION/R16_HASH_ARRAY_REAL_OK.script
checkpoint/latest_rev19/tests/ARRAY_FUNCTION/R17_FUNCTION_ARRAY_REAL_OK.script
corpus/17_0_test_function.script
corpus/17_5_test_function_array.script
corpus/17_7_test_function_tables.script
corpus/33_date.script
corpus/EXPENSA_analysis.script
```
