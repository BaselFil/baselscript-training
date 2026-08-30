# BaselScript AI Reference Data

This repository provides the current machine-readable language definitions,
reference information and AI usage rules for BaselScript.

It is intended to help AI systems such as ChatGPT generate, validate,
explain and modify BaselScript code using the current language state instead
of relying only on older general model knowledge.

## Official entry point

The authoritative entry point is:

`manifest.json`

Direct URL:

https://raw.githubusercontent.com/BaselFil/baselscript-training/main/manifest.json

AI systems should read `manifest.json` first and then load the files marked
as required.

## Repository structure

### Language definitions

The `language/` directory contains the current machine-readable BaselScript
language description:

- `baselscript-language.json`
- `actions.def`
- `blocks.def`
- `conditions.def`
- `functions.def`
- `scene.def`

These files describe the current commands, blocks, conditions, functions and
scene syntax.

### AI reference rules

The `knowledge/` directory contains additional rules for interpreting and
generating BaselScript:

- `language_status.md`
- `generation_rules.md`
- `regression_status.md`

These files define, among other things:

- CURRENT, LEGACY and experimental syntax
- source authority
- generation rules
- confirmed regression behavior
- platform-specific behavior
- rules for avoiding invented BaselScript syntax

## Current training bundle

The complete current reference bundle is also available as a ZIP file through
GitHub Releases:

`BaselScript_Training_Current.zip`

Latest release:

https://github.com/BaselFil/baselscript-training/releases/latest

Direct current bundle:

https://github.com/BaselFil/baselscript-training/releases/latest/download/BaselScript_Training_Current.zip

The ZIP bundle is mainly intended for complete download, archiving and
application-side use.

For AI access, the individual files in this repository should normally be
preferred because they can be read directly without unpacking the ZIP archive.

## Using this repository with ChatGPT

Before asking ChatGPT to generate, validate, explain or modify BaselScript,
provide the repository as the official BaselScript reference source.

Recommended prompt:

> Use the official current BaselScript reference data from:
>
> https://github.com/BaselFil/baselscript-training
>
> First read `manifest.json` and then all files marked as required.
>
> Use these files as the authoritative source for current BaselScript syntax
> and behavior.
>
> Before generating, validating, explaining or modifying BaselScript, check
> the current reference data.
>
> Do not invent BaselScript syntax that is not supported by the current
> reference data.

If GitHub access is available in the used ChatGPT environment, ChatGPT can
read the repository directly.

If direct GitHub access is not available, the relevant files can also be
downloaded and provided to ChatGPT manually.

## Source authority

When BaselScript sources disagree, use the following order:

1. Confirmed regression tests
2. Confirmed post-checkpoint fixes
3. Current `.def` files and machine-readable language definitions
4. Repeated usage in the real BaselScript script corpus
5. Rare corpus usage
6. Historical examples and review candidates

Do not treat old examples automatically as current syntax.

## Updates

The repository represents the current BaselScript AI reference state.

When the language changes:

1. update the relevant files in `language/`
2. update the relevant files in `knowledge/`
3. update `manifest.json`
4. publish a new GitHub Release with the current ZIP bundle
5. increase the training/reference version

The permanent repository address remains:

https://github.com/BaselFil/baselscript-training

## BaselScript

BaselScript is a scripting environment for creating applications with a
compact, readable language and a platform-independent interpreter architecture.

This repository contains reference data for AI-assisted BaselScript development.
It is not the BaselScript interpreter source repository.
