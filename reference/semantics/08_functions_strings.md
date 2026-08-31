# 08 - Functions / strings

Status: STRONGLY VERIFIED AT CATALOG LEVEL

Primary source:

- `language/functions.def`

## Universal call rule

```baselscript
#result=$function(...)
```

Function names are case-sensitive according to the current machine contract.

Prefer canonical names from `functions.def`.

## Strongly grounded string families

Examples include:

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
split_to_array
split_to_fields
substr_between
replace_between
```

Example:

```baselscript
#title=$concat("KEY ",#c1," ",#c2)

if $contains(#scriptname,".script")==0
    #scriptname=$add_extention(#scriptname,".script")
endif
```

## Aliases

Aliases are accepted only when listed in `functions.def`.

Do not create aliases by analogy.

## Arity

Respect the exact machine arity:

```text
2
2|3
1..4
*..2
2..*
*
```

`*` means the parameter-count rule has not yet been migrated into the machine contract; it is not permission to pass arbitrary invented arguments.

## Corpus-only / unverified examples

Do not generate spellings absent from the current function definition, including historically observed forms such as:

```text
as_it_is
date.format
draw.tile_color
exist_in_array
math.cosinus
math.sinus
string.rgb
string.rgb_hex
txt
upper_substring
```

unless future runtime verification promotes them.

## Generation procedure

1. find the function in `functions.def`;
2. use canonical spelling if practical;
3. keep `$`;
4. respect arity;
5. check runtime requirement;
6. consult the routed semantic category for side effects;
7. classify missing spellings as unverified instead of guessing.
