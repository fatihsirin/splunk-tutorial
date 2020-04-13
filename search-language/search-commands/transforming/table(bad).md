- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Table
# Background knowledge
- Use `table` to generate a table with only the specified field names and table columns
  - This command is *required* to properly format dashboard panel tables because `fields` doesn't show the field names for some reason
- It is useful for creating tables for dashboard panels
- It is useful for filtering output so that it can easily be used in a subsearch
# Arguments
## Required
- A list of comma-separated field names
  - E.g. `... | table current, line_id`
## Optional
- x
# Examples
- x