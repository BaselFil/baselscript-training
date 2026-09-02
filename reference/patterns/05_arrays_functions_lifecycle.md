# Arrays and functions lifecycle patterns

Status: CURRENT SEMANTICS + CORPUS-GROUNDED

## Function usage

Functions are expressions and use `$`.

```baselscript
#name_upper=$upper(#name)
#d=$date()
```

Do not turn functions into statement-level actions and do not turn actions into
`$function(...)` calls.

Exact function names, aliases and parameter counts come from `functions.def`. Return
semantics and side effects come from routed semantic files.

## One-dimensional array lifecycle

Current BaselScript arrays are one-dimensional.

Canonical declaration forms are documented by `07_arrays_hash.md`. Typical usage is:

```baselscript
array name=#values[3] value=(100,200,300)

#first=#values[0]
```

Indexing begins at 0.

## File to arrays

This cross-domain pattern also requires the `files_data` route.

A common data-processing pipeline is:

```text
declare structured file
-> read file
-> use generated field arrays
-> process values
```

Example from current reference material:

```baselscript
SCENE=1

SECTION init
    file name=polygon record=(#xx,#yy) dir=#_directory_files_examples
    read file=polygon dir=#_directory_files_examples

    #x0=#xx_array[0]
    #y0=#yy_array[0]
END

END SCENE
```

Use the exact generated-array convention documented by current file semantics. Do not
invent a different collection object model.

## Array iteration

Current runtime/regression supports section-based FOREACH over BaselScript arrays:

```baselscript
array name=#names[3] value=("Anna","Peter","Maria")

foreach #name in #names[] sec=show_name

SECTION show_name
    trace #name
END
```

The source is an existing BaselScript array. FOREACH does not mean generic object,
SQL-row or dictionary iteration.

## FOR with arrays

The corpus also contains index-based loops. Use the current control-flow semantics for
the exact FOR syntax and bounds.

Conceptual pattern:

```text
prepare array
-> prepare counter/index
-> FOR ... sec=<body>
-> body reads/writes array[index]
```

Do not import `for (...) {}` syntax from another language.

## Hash-array lifecycle

Hash-array operations exist in current scripts and semantic documentation.

Typical responsibility chain:

```text
clear target hash array
-> set key/value pairs
-> lookup/process
```

Because old corpus material contains spelling and alias variation, new code must use
the canonical current form documented by `07_arrays_hash.md` and `actions.def`.

## Side-effect rule

Some functions initialize runtime state in addition to returning a value.

Example:

```baselscript
#d=$date()
message #_current_weekday_name
```

Do not infer such side effects from names. Use only those documented by the relevant
semantic route.
