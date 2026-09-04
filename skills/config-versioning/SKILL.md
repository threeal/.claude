---
name: config-versioning
description: Language/framework-agnostic guidance for designing config systems that stay backward compatible across code changes, and for writing config parsers that anticipate future versions instead of hardcoding the current one. TRIGGER - about to design a config file/schema, add persisted settings/state, or write/modify config-loading or config-migration logic. SKIP - for validating config values against business rules unrelated to version compatibility.
---

## Suggest Versioning When Designing a Config System

When implementing a config system (a file, schema, or any persisted state that outlives a single release), suggest an explicit version field/requirement as the mechanism for determining backward compatibility. Offer it as a suggestion — it's the user's call whether to adopt it, not a mandate.

**Why**: code changes, but persisted config is not disposable — a user's existing config represents state they've already invested in. Prefer being able to keep supporting an old config over forcing a reset whenever the code changes. An explicit version marker is what makes that feasible: it lets parsing code know, up front, which shape a given config is in and how to interpret it, instead of guessing from which fields happen to be present or absent.

## Never Parse Versions With a Static Equality Check

Even when only one config version currently exists, implement the parsing/loading logic as an open dispatch over version, not a static assertion against a single supported version. This encodes the fact that future versions are expected, and keeps the code shaped to receive them.

```
# Wrong — hardcodes the assumption that only one version will ever exist
if config.version != CURRENT_VERSION:
    raise Error("unsupported config version")
parse_current(config)
```

```
# Right — dispatches per version, and stays extensible as new versions appear
match config.version:
    case "1":
        parse_v1(config)
    case "2":
        parse_v2(config)
    case _:
        raise Error(f"unsupported config version: {config.version}")
```

This applies regardless of language or framework — a `switch`/`match` statement, a map of version → handler function, or a chain of version-specific parser classes are all valid shapes for the same idea. The point is that adding a new version means adding a case, not editing a condition.

## Handle Missing or Changed Data Per Field, Not All-or-Nothing

The current implementation expects some set of data/state to come out of config. An older version's config may not be able to provide all of it. When a version-specific parser hits data the current code needs but that version doesn't carry, there are several legitimate resolutions — pick whichever fits the field, in roughly this order of preference:

- fall back to a sensible default value
- derive/calculate it from other values that are present
- refuse — only when the missing data is genuinely required and can't be recovered any other way

## Incompatibility Can Be Partial

An old or unrecognized version doesn't automatically mean the whole config is rejected. Often only one specific field blocks the current implementation from knowing one specific piece of data, while the rest of the config is still perfectly usable. Whether that gets surfaced as a warning, an error, a log line, or is silently absorbed via a default depends on how critical that field's data is to the implementation — decide it in context of the concrete field, don't reflexively fail the whole parse over a single incompatible field when the rest of the config is fine.
