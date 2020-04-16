- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Stats - `stats` documentation
- https://answers.splunk.com/answers/32001/difference-between-stats-and-chart.html
# Background
- `chart` and `stats` can return identical output, but it depends on the exact search
  - Deciding between them depends on the desired output format
- Use the `stats` command when I want apply a statistical function to complex groups formed with multiple group-by fields
# Arguments
## Required
- One *or more* statistical functions
  - Each statistical function is independent; they do not affect each other
## Optional
- I cannot specify a split-by clause with `stats`, unlike `chart`
- A *group-by* (i.e. `BY`) clause which can accept one *or more* comma-separated fields to *group-by*
  - The *names* of the group-by fields will be table columns in the resulting table
  - Each unique *combination* of group-by field values will form a row, and the statistical function(s) will be applied separately to each group of
    events that possess the same unique combination of group-by field values
    - E.g. grouping by two fields will create a \<Number of distinct field 1 values> * \<Number of distinct field 2 values> number rows, and each row
      will have the statistical function(s) applied to it
# Examples
## Group by three fields
- `sourcetype="scada" | rex field="Power _MVA" "(?<pow>\d+)" | eval "Power _MVA"='pow' | stats count by "origin_bus_id", "destination_bus_id", "Line ID"`
```
--------------------------------------------------------
| origin_bus_id | destination_bus_id | Line ID | count |
--------------------------------------------------------
| 3             | 6                  | 3-6     | 96    |
--------------------------------------------------------
| 4             | 5                  | 4-5     | 96    |
--------------------------------------------------------
| 4             | 9                  | 4-9     | 96    |
--------------------------------------------------------
```
- For the 864 returned events, truncate each value of the "Power _MVA" field to its integer digits. Then, perform the `count` statistical function
  within each group. Events are grouped according to sharing a particular combination of the "origin_bus_id", "destination_bus_id", "Line ID" field
  values
- Return the table of statistics and/or a visualization