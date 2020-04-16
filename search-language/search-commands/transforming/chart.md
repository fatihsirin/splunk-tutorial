- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Chart - `chart` documentation
- https://answers.splunk.com/answers/32001/difference-between-stats-and-chart.html
# Background
- `chart` and `stats` can return identical output, but it depends on the exact search
  - Deciding between them depends on the desired output format
- Use the `chart` command when I want to apply a statistical function granulalry to a single field
# Arguments
## Required
- One *or more* statistical functions
  - Each statistical function is independent; they do not affect each other
## Optional
- A split-by (i.e. `BY`) clause that accepts a single field to split by
  - The unique *values* of the single split-by field will be table columns in the resulting table
- A group-by (i.e. `OVER`) clause that accepts a single field to group by
  - Each unique *value* of the group-by field will create its own row
# Examples
## Identical output from separate commands
- The `sourcetype="scada" | chart count as line_count over "Line ID"` command and the `sourcetype="scada" | chart count as line_count by "Line ID"`
  command return the same result. Why?
- 
## Use group-by and split-by fields
- `sourcetype="scada" | rex field="Power _MVA" "(?<pow>\d+)" | eval "Power _MVA"='pow' | chart count over "Power _MVA" by "Line ID"`
```
--------------------------------------------------------------------
| Power _MVA | 1-4 | 2-8 | 3-6 | 4-5 | 4-9 | 5-6 | 6-7 | 7-8 | 8-9 |
--------------------------------------------------------------------
| 0          | 0   | 0   | 0   | 1   | 0   | 1   | 0   | 0   | 0   |
--------------------------------------------------------------------
| 1          | 0   | 0   | 0   | 1   | 0   | 0   | 0   | 0   | 0   |
--------------------------------------------------------------------
```
- For the 864 returned events, truncate each value of the "Power _MVA" field to its integer digits. Then, perform the `count` statistical function
  within each group. Each group is a unique value of the "Power _MVA" field, hence "Power _MVA" is the group-by field. Since there is a split-by
  field, "Line ID", within each group we split the count according to the value of "Line ID"  
- Return the table of statistics and/or a visualization
## Copy the `timechart` command
- `sourcetype="scada" | chart first("Origin Voltage _PU") over _time by "Line ID"` and 
  `sourcetype="scada" | timechart span=15m first("Origin Voltage _PU") by "Line ID"` generate the same output