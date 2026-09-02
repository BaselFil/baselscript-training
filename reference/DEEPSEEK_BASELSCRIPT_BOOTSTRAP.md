# BaselScript knowledge loader for DeepSeek

Назначение

Этот файл является входной картой знаний BaselScript для DeepSeek.
DeepSeek должен читать источники только по прямым raw-URL, указанным ниже.
Не заменять raw-URL ссылками на обычные страницы GitHub.

## Главное правило

Перед генерацией, проверкой, объяснением или изменением BaselScript:

- сначала прочитать все ссылки из раздела BASELINE
- определить все подходящие категории TASK ROUTES
- применить связанные ROUTE EXPANSIONS
- прочитать объединение всех URL из выбранных маршрутов
- только после этого генерировать или исправлять BaselScript
- не придумывать синтаксис, команды, параметры или семантику, которых нет в загруженных источниках
- при противоречии считать более авторитетным текущий runtime/regression, затем machine contract, затем semantics, затем patterns
- evidence и regression не загружать по умолчанию, использовать только для разрешения сомнений или проверки спорной формы
- baselscript-language.json не загружать по умолчанию, кроме ui_defaults, machine_full или общей fallback-проверки

## BOOTSTRAP

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/manifest.json
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/AI_CONTEXT.md

## BASELINE

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/manifest.json
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/AI_CONTEXT.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/knowledge/generation_rules.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/00_INDEX.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/01_core_language.md

## TASK ROUTES

### structure

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/02_structure.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/01_structure_execution.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def

### call_execution

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/03_call_execution.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/01_structure_execution.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def

### control_flow

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/04_control_flow.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/08_control_flow_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/conditions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### ui

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### form

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/19_form.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def

### list

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/21_list.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### dialog

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/20_dialog.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### files_data

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### arrays_hash

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/07_arrays_hash.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/05_arrays_functions_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### functions_strings

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/08_functions_strings.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/05_arrays_functions_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### math

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/09_math.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### date_time

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/10_date_time.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### database_sql

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/11_database_sql.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/04_database_sql_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### security_crypto

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/12_security_crypto.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/07_security_crypto_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### graphics

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/13_graphics.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/06_graphics_charts_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### charts

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/14_charts.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/06_graphics_charts_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### network_media_device

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/15_network_media_device.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/09_network_device_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### script_meta

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/16_script_meta.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/10_script_meta_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### testing

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/17_testing.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/08_control_flow_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/conditions.def

### platform

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/18_platform.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### ui_defaults

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/baselscript-language.json

### machine_full

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/conditions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/baselscript-language.json

## ROUTE EXPANSIONS

### complete_script

- routes: structure, call_execution

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/02_structure.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/01_structure_execution.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/03_call_execution.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def

### file_backed_list

- routes: files_data, list

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/21_list.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def

### file_backed_form

- routes: files_data, form

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/19_form.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def

### adaptive_form

- routes: form, platform

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/19_form.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/18_platform.md

### adaptive_list

- routes: list, platform

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/21_list.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/18_platform.md

### file_backed_adaptive_form_and_list

- routes: files_data, form, list, platform, structure, call_execution

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/19_form.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/21_list.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/18_platform.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/02_structure.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/01_structure_execution.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/03_call_execution.md

### sql_rows_displayed_as_list

- routes: database_sql, files_data, list

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/11_database_sql.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/04_database_sql_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/05_ui.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/21_list.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/02_ui_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def

### chart_from_file

- routes: charts, files_data

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/14_charts.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/06_graphics_charts_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def

### graphics_from_file

- routes: graphics, files_data

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/13_graphics.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/06_graphics_charts_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/06_files_data.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/03_files_data_lifecycle.md

### graphics_from_array

- routes: graphics, arrays_hash

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/13_graphics.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/06_graphics_charts_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/07_arrays_hash.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/05_arrays_functions_lifecycle.md

### encrypted_application_database

- routes: database_sql, security_crypto

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/11_database_sql.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/04_database_sql_lifecycle.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/semantics/12_security_crypto.md
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/patterns/07_security_crypto_lifecycle.md

## MACHINE CONTRACT

- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/functions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/actions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/blocks.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/conditions.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/scene.def
- https://raw.githubusercontent.com/BaselFil/baselscript-training/main/reference/language/baselscript-language.json

## DEEPSEEK OPERATING INSTRUCTION

Используй этот порядок работы:

1. Открой BOOTSTRAP.
2. Открой весь BASELINE.
3. Классифицируй запрос пользователя по всем подходящим TASK ROUTES.
4. Для полного исполняемого скрипта всегда добавляй complete_script.
5. Для FORM, LIST, файлов, SQL, графики и платформенных особенностей добавляй все соответствующие маршруты, а не только один.
6. Если применима ROUTE EXPANSION, загрузи все входящие в нее URL.
7. Не используй знания о синтаксисе BaselScript из общих предположений или других языков.
8. Если форма не подтверждена источниками, явно сообщи, что она не подтверждена, вместо выдумывания.
9. Если задача требует полной машинной проверки, используй machine_full.
10. Если задача требует только UI defaults, загружай baselscript-language.json через ui_defaults.

## EXAMPLE ROUTING

Запрос: создать файл-backed LIST

- BASELINE
- files_data
- list
- complete_script

Запрос: создать адаптивную FORM с файлом

- BASELINE
- files_data
- form
- platform
- complete_script

Запрос: проверить весь язык машинно

- BASELINE
- machine_full

## SOURCE

- Repository: BaselFil/baselscript-training
- Branch: main
- Reference manifest version at preparation: clean-v1-2026-09-02
