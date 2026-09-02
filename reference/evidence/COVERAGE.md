# Coverage status

This clean reference intentionally separates machine coverage from semantic coverage.

## Strong machine coverage

- function catalog: canonical names, aliases, arity, requirements
- action catalog: names, match mode, selected syntax metadata, runtime requirements
- structural blocks
- SCENE grammar metadata
- condition keywords/operators
- generated Windows UI defaults

## Semantic areas already anchored by verified project behavior

- `$` function invocation
- `#` variable form
- current weekday-name state via `$date()` and `#_current_weekday_name`
- core graphics circle pattern
- distinction between ApplicationDatabase and SystemDatabase actions
- Prefix ordering requirement for overlapping actions

## Semantic areas still requiring audit

- full action parameter grammar for actions whose syntax field is empty
- complete CALL target/resolution matrix
- complete UI tile grammar and dynamic UI
- complete file/data action grammar
- array declaration/indexing/mutation grammar
- function return types and side effects beyond verified cases
- full graphics primitive grammar
- chart parameters and chart-type semantics
- network/media/device runtime behavior
- script/meta side effects
- Android-specific platform behavior

An incomplete semantic category must remain incomplete rather than being filled with guessed syntax.
