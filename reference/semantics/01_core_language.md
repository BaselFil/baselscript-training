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

## 15. Physical lines and line continuation

Status: VERIFIED BY CURRENT VALIDATOR / REGRESSION

A BaselScript statement is normally written on one physical source line.

### Recommended line length

As a readability rule, keep a BaselScript statement on one physical line when it is approximately **80 characters or shorter**.

The 80-character value is a **style recommendation**, not a validator or runtime syntax limit.

Do not split a statement only for visual formatting when it fits comfortably on one line.

Preferred:

```baselscript
message $concat(#first_name," ",#last_name," / ",#city)
```

Avoid unnecessary formatting such as:

```text
message $concat(
    #first_name," ",
    #last_name," / ",
    #city)
```

Parentheses do **not** automatically continue a BaselScript statement onto the next physical line.

### Use `\` when a statement is continued

When a statement becomes clearly longer than about 80 characters, or when splitting it materially improves readability, it may be distributed across multiple physical source lines.

Every physical line that continues onto the following line must end with the continuation character `\`.

Canonical multiline form:

```baselscript
message $concat( \
    #first_name," ", \
    #last_name," / ", \
    #city)
```

The last physical line of the logical statement does not need a continuation character because the statement ends there.

Another example:

```baselscript
#text=$concat( \
    #first_name," ", \
    #last_name)
```

Wrong:

```text
#text=$concat(
    #first_name," ",
    #last_name)
```

The validator may interpret the following physical line as a new statement because no continuation marker was present.

### AI generation rule

For newly generated BaselScript:

1. keep a statement on one physical line when it is approximately 80 characters or shorter;
2. do not introduce line breaks merely because another programming language would format arguments vertically;
3. if a longer statement is split across physical lines, end every continued physical line with `\`;
4. do not rely on parentheses, commas or indentation as implicit continuation syntax;
5. treat the 80-character value as a readability guideline, not as a syntax restriction.

Existing source may contain compatibility or historical continuation forms. Preserve working historical code when reviewing an existing script, but prefer `\` in newly generated multiline BaselScript.

---

## 16. Source-of-truth rule

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

## 17. Do not invent syntax

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

## 18. Scope limits of this file

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
