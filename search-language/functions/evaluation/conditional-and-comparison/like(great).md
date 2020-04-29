# Examples
## Wildcard matching with %
### Search
```
index="electrical_utility_data" | where like (_raw, "%Goodbye World!%")
```
- The "%" character is the multi-character wildcard within a `LIKE` pattern
  - The wildcard matches zero or more characters
## Character matcing with _
### Search
```
index="electrical_utility_data" | where like (_raw, "%Goodbye World_")
```
- The "_" character is the single-character wildcard within a `LIKE` pattern
  - The wildcard matches exactly one character
# Purpose
- The `LIKE` operator returns true if the given pattern exists with the specified text
# Syntax
- `LIKE (<text or field>, <pattern>)`
# Performance
- I'm guessing that the `LIKE` operator is less performant then the `IN` operator because `LIKE` can use wildcards
  - Or perhaps `LIKE` calls `IN` when no wildcards are supplied? (I'm speculating)