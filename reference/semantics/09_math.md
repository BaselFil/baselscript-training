# 09 - Math

Status: VERIFIED AT FUNCTION-CATALOG LEVEL

Primary source:

- `language/functions.def`

Current families include:

```text
math.abs
math.abs_diff
math.sin
math.sinh
math.cos
math.cosh
math.sec
math.sech
math.cosec
math.cosech
math.tan
math.tanh
math.cotan
math.coth
math.arcsin
math.arccos
math.sqrt
math.cbrt
math.log
math.log10
math.log2
math.min
math.max
math.random
math.round
math.round_down
math.round_up
math.truncate
math.factorial
math.odd
math.is_divisible_by_n
pow
to_radians
to_degrees
```

Canonical function use:

```baselscript
#max=$math.max(10,20)
#root=$math.sqrt(81)
#p=$pow(2,8)
```

Respect the declared arity.

Do not silently replace a defined canonical name with an English synonym.

Do not promote corpus-only historical spellings such as `math.cosinus` or `math.sinus`.

Numeric formatting and conversion semantics belong to the specific function behavior, not to this generic math category.
