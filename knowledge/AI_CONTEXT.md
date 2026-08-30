# BASELSCRIPT_AI_CONTEXT

Version: 0.1
Status: draft

## Purpose

Этот файл является постоянным контекстом для AI, который создаёт или изменяет BaselScript-скрипты.
Он должен использоваться вместе с актуальным `system/baselscript-language.json`.

AI не должен придумывать синтаксис, отсутствующий в language contract или подтверждённых examples.

## Source priority

При конфликте источников использовать такой приоритет:

1. `system/*.def` и UI parameter files;
2. `system/baselscript-language.json`;
3. `knowledge/rules/rules.json`;
4. stable patterns;
5. golden examples;
6. complete reference applications;
7. предположение AI — только если выше нет ответа.

## Generation model

```text
USER REQUEST
    ↓
language contract
    ↓
semantic rules
    ↓
select patterns
    ↓
select examples
    ↓
generate .script
    ↓
ScriptValidator
    ↓
runtime
```

## Hard runtime semantics

### Startup scene

Первая исполняемая сцена приложения всегда:

```baselscript
SCENE=1
```

Именованные сцены использовать после стартовой.

### Multi-step form state

В одном workflow значения, введённые пользователем, должны сохраняться при переходах `Next` и `Back`.

Нельзя очищать поля при обычном переходе между шагами.

Инициализация выполняется только при начале нового workflow.

Сброс состояния выполняется после успешного `Save`, явного `New` или явного завершения workflow.

### Conditions

- nested IF не поддерживается;
- `&&` допустим;
- `||` допустим;
- наружные скобки вокруг всего IF-условия не использовать.

### References

- SECTION/ACTION используют общий callable namespace;
- callable reference может ссылаться на цель в другой scene;
- `sec=none` / `act=none` — legacy no-op только для SECTION/ACTION.

## UI generation

Windows UI defaults находятся в:

```text
system/_parameters_portrait_win
system/_parameters_landscape_win
```

и экспортируются в `baselscript-language.json`.

Для обычных `tile=text`, `tile=input`, `tile=button` AI должен предпочитать system defaults и не задавать без необходимости:

```text
h
s
style
gravity
```

Явный `w`/`h` допускается, когда layout требует специальной геометрии.

Для form generation учитывать `field_label_gap`, если он присутствует в UI contract:

```text
input_y = label_y + label_height + field_label_gap
```

## Patterns

Pattern отвечает на вопрос: как правильно собрать типовую задачу средствами BaselScript.

Pattern не должен быть привязан к конкретной предметной области (`person`, `patient`, `customer`) или к конкретному имени переменной.

Статусы pattern:

```text
draft
candidate
stable
deprecated
```

AI по умолчанию должен использовать `stable`.

## Examples

Example показывает конкретную реализацию pattern.

Статусы example:

```text
candidate
golden
deprecated
```

`golden` означает:
- проходит ScriptValidator;
- реально запускается;
- подтверждён runtime-тестом;
- признан правильным эталоном.

AI должен предпочитать `golden`.

## Validation

AI-generated `.script` не должен считаться готовым до прохождения ScriptValidator.

При ошибках Validator:
1. передать AI исходный script;
2. передать diagnostics;
3. исправить script;
4. снова выполнить validation.

## Windows / Android

Язык BaselScript должен иметь один общий контракт.

Платформенно-зависимыми могут быть:
- загрузка файлов/assets;
- runtime implementation;
- UI parameter files;
- platform APIs.

Семантика `.script` должна оставаться одинаковой.

## Knowledge Package

Для нового AI-сеанса достаточно передать каталог/ZIP `FOR_AI` и попросить:

> Сначала изучи manifest.json и указанные в нём файлы. Используй только актуальный BaselScript contract, rules, patterns и examples. Не придумывай синтаксис вне этих источников.
