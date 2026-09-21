---
name: config-versioning
description: Language/framework-agnostic guidance for designing config systems that stay backward compatible across code changes, and for writing config parsers that anticipate future versions instead of hardcoding the current one. TRIGGER - about to design a config file/schema, add persisted settings/state, or write/modify config-loading or config-migration logic. SKIP - validating config values against business rules unrelated to version compatibility.
---

## Suggest Versioning When Designing a Config System

When implementing a config system — a file, a schema, any persisted state that outlives a release — suggest an explicit version field as the mechanism for backward compatibility. Offer it; whether to adopt it is the user's call, not a mandate.
Why: persisted config isn't disposable, it's state the user already invested in, so prefer keeping old configs working over forcing a reset whenever the code changes. An explicit version marker lets parsing code know up front which shape it's holding, instead of guessing from which fields happen to be present.

## Never Parse Versions With a Static Equality Check

Even when only one version exists, write the loader as an open dispatch over version, not an assertion against a single supported value.
Why: this encodes that future versions are expected, so adding one means adding a case rather than editing a condition.

Incorrect:

```
if config.version != CURRENT_VERSION:
    raise Error("unsupported config version")
parse_current(config)
```

Correct:

```
match config.version:
    case "1":
        parse_v1(config)
    case "2":
        parse_v2(config)
    case _:
        raise Error(f"unsupported config version: {config.version}")
```

## Handle Missing Data Per Field, Not All-or-Nothing

An older config may not carry everything the current code expects. When a version-specific parser hits data it can't get, resolve it per field, in rough order of preference:

- fall back to a sensible default
- derive it from other values that are present
- refuse — only when the data is genuinely required and unrecoverable

Incompatibility is usually partial: one field may block one piece of data while the rest of the config stays perfectly usable. Whether that surfaces as a warning, an error, a log line, or is absorbed silently depends on how critical that field is — decide per field rather than failing the whole parse over one of them.
