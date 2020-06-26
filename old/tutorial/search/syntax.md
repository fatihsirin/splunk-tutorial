- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Startsearching
# Logical operators
- If I type multiple keywords into the search bar, the AND, OR, and NOT logical operators can be used
- The boolean operators must be capitalized
## Precedence
- Precedence is given to items inside of parentheses
- NOT takes precedence over OR
- AND has the lowest precedence
## Operators
### AND
- The AND operator is implicit
    - E.g. `buttercupgames error` is the same as typing `buttercupgames AND error`
### OR
- The OR operator is not implicit and should be used with parentheses
    - E.g. `buttercupgames (error OR fail* OR severe)` will search for `buttercupgames` in conjunction with `error` or `fail` or `failure` or
      `failed` or `severe`
# Wildcard matching
- The `*` character performs wildcard matching
    - E.g. `fail*` matches `fail`, `failure`, `failed`, etc.
# Fields
- To search events based on fields, use the `<field>=<value>` syntax
    - E.g. `sourcetype=access_*`
- Field names are case-sensitive, but field values are not
- Wildcards can be used in field values
- Quotation marks are required when field values include spaces
# Quoted phrases
- Quoted phrases must appear in their entirety in a matching event
    - E.g. `"database error"` will return different results than `database error`
# Commands
- A search can be piped into a Splunk command to transform, filter, and report on events
    - E.g. `sourcetype=access_* status=200 action=purchase | top categoryId` pipes a set of matching events into a command that returns the most
      common `categoryId` field values
- The output of a command can be piped into another command
## Transforming commands
- Transforming commands organize the search results into a table in the Statistics tab
    E.g. `top`, `chart`
- Many transforming commands return additional fields that are useful