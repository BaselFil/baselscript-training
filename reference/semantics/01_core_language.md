# 01 - Core language

Status: PARTIALLY VERIFIED

This file defines source-level rules that apply across BaselScript categories.

It does not replace the machine contract. The machine contract remains:

- `language/actions.def`
- `language/functions.def`
- `language/conditions.def`
- `language/blocks.def`
- `language/scene.def`
- `language/baselscript-language.json`

The machine contract defines known language elements. This file defines how those elements are written and interpreted at source level where that behavior has been verified.

---

## 1. Variables

Status: VERIFIED BY CURRENT RUNTIME / REAL SCRIPTS

BaselScript variables use the `#` prefix.

Canonical form:

```baselscript
#name="BaselScript"
#count=10
#result=#count
```

System/runtime variables also use `#`.

System variables commonly use names beginning with:

```text
#_
```

Example:

```baselscript
#_current_weekday_name
```

Do not generate user variables without `#`.

Wrong:

```text
name="BaselScript"
result=count
```

---

## 2. Assignment

Status: VERIFIED BY CURRENT RUNTIME / REAL SCRIPTS

Assignment uses:

```text
=
```

Canonical form:

```baselscript
#variable=value
```

Examples:

```baselscript
#count=10
#name="BaselScript"
#other=#count
```

Important:

`=` is assignment syntax.

It is not exposed by the current condition contract as a semantic equality operator.

For equality comparison use:

```text
==
```

---

## 3. Function calls

Status: VERIFIED BY CURRENT RUNTIME / REAL SCRIPTS

BaselScript functions use the `$` prefix when called.

Canonical form:

```baselscript
$function_name(...)
```

When the result is assigned:

```baselscript
#result=$function_name(...)
```

Examples:

```baselscript
#d=$date()
#max=$math.max(10,20)
#text=$upper(#name)
```

Do not generate:

```text
date()
math.max(10,20)
#d=date()
#max=math.max(10,20)
```

`functions.def` defines:

- canonical function name
- aliases
- allowed parameter count
- runtime requirement

`functions.def` does not by itself define every return value, side effect or source-level usage pattern.

---

## 4. Function names and aliases

Status: VERIFIED BY MACHINE CONTRACT

Prefer the canonical function name from `functions.def`.

Example machine entry:

```text
date;date.current_date|current_date;*..4;None
```

Preferred generated form:

```baselscript
#d=$date()
```

Aliases may be accepted by the runtime, but generated BaselScript should normally use the canonical name unless a routed semantic file documents a preferred public alias.

Do not invent aliases.

---

## 5. Actions

Status: VERIFIED BY MACHINE CONTRACT

Actions are statement-level commands.

Current action names are defined in `actions.def`.

Examples include:

```text
message
echo
draw
read
write_record
call
db_use
```

An action is not called with `$`.

Example:

```baselscript
message "Hello"
```

Do not generate:

```text
$message("Hello")
```

The existence of an action in `actions.def` proves that the action name is recognized by the current contract.

It does not prove an undocumented parameter syntax.

If an action has no documented `syntax` metadata, consult the semantic file for its category before generating parameters.

---

## 6. Function versus action

Status: VERIFIED LANGUAGE RULE

These are different language constructs.

Function:

```baselscript
#d=$date()
```

Action:

```baselscript
message #d
```

Do not transform an action into function-call syntax.

Do not transform a function into action syntax.

Wrong:

```text
date
$message(#d)
```

---

## 7. Conditions

Status: VERIFIED BY MACHINE CONTRACT

The current semantic condition operators are:

```text
==
!=
>
<
>=
<=
contains
><
<>
&&
&
||
```

The current condition contract does not expose these as semantic operators:

```text
=
!
|
```

Example:

```baselscript
if #a == #b
    message "equal"
endif
```

Do not generate:

```text
if #a = #b
```

for equality comparison.

---

## 8. IF structure

Status: VERIFIED BY MACHINE CONTRACT

Current condition keywords are:

```text
if
elseif
else
endif
```

`if` and `elseif` require an expression.

Example:

```baselscript
if #value == 1
    message "one"
elseif #value == 2
    message "two"
else
    message "other"
endif
```

---

## 9. Structural blocks

Status: VERIFIED BY MACHINE CONTRACT

The current machine contract recognizes:

```text
SECTION
ACTION
MENU
LIST
DIALOG
FORM
POPUPMENU
```

SCENE has separate grammar.

Important:

values such as:

```text
sec
act
dia
```

inside `blocks.def` are validator matching tokens.

They are not canonical names that AI should generate.

Generate:

```baselscript
SECTION init
...
END
```

not:

```text
sec init
```

Detailed block grammar belongs to `02_structure.md`.

---

## 10. SCENE

Status: VERIFIED BY MACHINE CONTRACT

SCENE is a special structural construct.

The machine contract defines:

```text
start keyword = scene
value separator = =
end prefix = end
end contains = scene
implicit next start = true
implicit EOF = true
name required = true
```

Canonical SCENE examples and optional parameters belong to `02_structure.md`.

Do not infer additional SCENE parameters from `scene.def` alone.

---

## 11. Strings

Status: VERIFIED BY REAL SCRIPTS

String literals use quotation marks in verified BaselScript source.

Examples:

```baselscript
#name="BaselScript"
message "Hello"
```

Do not introduce alternate string literal syntax from another programming language unless explicitly documented.

---

## 12. Numbers

Status: VERIFIED BY REAL SCRIPTS

Numeric values may be used directly.

Examples:

```baselscript
#x=10
#y=20
```

Exact decimal-format and numeric-conversion rules belong to the relevant semantic/function documentation.

Do not infer them from another language.

---

## 13. System variables

Status: PARTIALLY VERIFIED

System variables are runtime-provided variables.

Their exact names and semantics must be documented by the semantic category that creates or consumes them.

Verified date/time example:

```baselscript
#d=$date()
message #_current_weekday_name
```

Do not invent system variables based only on naming patterns.

---

## 14. Side effects

Status: PARTIALLY VERIFIED

A function may do more than return a value.

Verified example:

```baselscript
#d=$date()
```

also initializes current-date runtime state used by system variables such as:

```text
#_current_weekday_name
```

Such behavior must be documented explicitly in the relevant semantic file.

Do not infer side effects from the function name alone.

---

## 15. Source-of-truth rule

Status: NORMATIVE REFERENCE RULE

The machine contract answers:

```text
Does this construct exist?
What is its canonical name?
What aliases exist?
How many parameters are accepted?
Does it require a specific runtime subsystem?
```

Semantic files answer:

```text
How is it written in source code?
What does it return?
What state does it change?
What system variables does it affect?
What combinations are valid?
Are there platform differences?
```

Both layers are required for reliable AI generation.

---

## 16. Do not invent syntax

Status: NORMATIVE REFERENCE RULE

When a requested operation is not fully documented:

1. check the relevant semantic category
2. check the machine contract
3. check verified regression/runtime evidence
4. if still unknown, say that the current reference does not document the source-level form

Do not substitute syntax from:

- C#
- Java
- JavaScript
- Python
- SQL dialects
- shell languages
- another scripting language

---

## 17. Scope limits of this file

This file intentionally does not define:

- complete array declaration/indexing grammar
- complete hash-array grammar
- complete file-action parameter grammar
- complete UI tile grammar
- complete CALL target matrix
- complete graphics grammar
- complete chart grammar
- complete platform-specific behavior

Those belong to their routed semantic categories.

---

## 8. Comments

Status: VERIFIED BY CURRENT NORMALIZATION / RUNTIME PIPELINE

Current source preprocessing recognizes comments before validation/execution.

Supported forms include:

```baselscript
// whole-line comment

#value=10 // trailing comment

/*
multi-line
block comment
*/
```

Current normalization also preserves comment markers that occur inside quoted text rather than treating them as comments.

Generation rule:

- use short `//` comments for ordinary generated examples;
- use comments to explain a rule, constraint, reason or non-obvious boundary;
- do not rely on comments to repair invalid syntax.

---

## 9. Physical-line continuation

Status: VERIFIED BY CURRENT NORMALIZER / REAL SCRIPTS

BaselScript source can combine several physical lines into one logical instruction.

### Backslash continuation

The normal current style for long statements is a trailing backslash:

```baselscript
SQL_SELECT id,first_name,last_name \
    FROM persons \
    ORDER BY id DESC \
    OUTPUT(#_directory_files,PERSONS_TEMP.csv)
```

The backslash is a source continuation marker. It is not part of the final logical instruction.

### Tilde continuation

The current normalizer also supports a trailing:

```text
~
```

for continuation.

This is a supported compatibility/source form. Do not replace a working `~` continuation automatically.

### Angle continuation

The current normalizer also retains the historical `>` continuation family, including source fragments shaped as:

```text
first>
>middle>
>last
```

Treat this as compatibility syntax. For new generated multi-line examples prefer the established backslash form unless the target project uses another verified style.

Generation rules:

- never invent a continuation symbol;
- preserve the existing working continuation style when editing a script;
- for new reference examples, prefer `\` where a statement must span physical lines.

---

## 10. Logical source before validation

Status: VERIFIED BY CURRENT NORMALIZATION PIPELINE

Validation and execution operate on normalized logical source, not blindly on the original physical lines.

Relevant preprocessing includes:

```text
physical source
-> line continuation normalization
-> comment removal
-> logical source
-> validator
-> runtime
```

This distinction matters when diagnosing a reported line: the user-facing source must remain traceable even when several physical lines form one executable instruction.

