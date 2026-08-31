# 07 - Arrays / hash arrays

Status: VERIFIED CORE, DECLARATION DETAILS PARTIAL

Primary source:

- `language/functions.def`

Current array function families include:

```text
array.counter
array.min
array.max
array.sum
array.count_of_string
array.count_of_value
array.clear
array.contains
array.add
array.arrays_are_equell
array.get_first_index
```

Current hash-array helpers include:

```text
hash_array.get_value
hash_array.length
```

## Function invocation

Examples:

```baselscript
#max=$array.max(#values)
#sum=$array.sum(#values)
```

Always keep `$`.

## Indexed arrays

Regression evidence confirms indexed arrays and interaction with FOR loops.

Do not invent a JavaScript/Python array literal or indexing form unless the exact BaselScript pattern is documented by a verified example.

## Hash arrays

Hash-array clear/set/get behavior is supported by real/regression evidence, but the complete declaration/mutation grammar is not represented in `functions.def`.

Use only current project examples for mutation syntax.

## Generation rules

- Use only function spellings present in `functions.def`.
- Do not generate `exist_in_array` unless a future machine contract confirms it.
- Do not infer generic methods such as `.push()`, `.map()`, `.filter()`.
