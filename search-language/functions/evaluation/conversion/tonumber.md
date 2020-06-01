- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/ConversionFunctions#tonumber.28NUMSTR.2CBASE.29
# Examples
## Convert a string literal into a number
### Search
```
`sourcetype="scada" | where 'Power _MVA'=tonumber("195.21081151654957000000")`
```
```
----------------------------------------------------------------------------------
| timestamp                 | line_id | power              | origin_bus_id | ... |
----------------------------------------------------------------------------------
| 2019-01-01 21:00:00+00:00 | 1-4     | 195.21081151654957 | 1.0           | ... |
----------------------------------------------------------------------------------
```
- Filter the 864 returned events to those whose "Power _MVA" field equals the numeric literal
  - Notice how the `tonumber` function can go on the right-hand side of the `eval` expression
- Return the one matching event
  - The `tonumber` function is not redundant in this example because the numeric literal is enclosed in quotations
## Convert a field to a number
```
sourcetype="scada" | where tonumber('Power _MVA')=195.21081151654957000000
```
```
----------------------------------------------------------------------------------
| timestamp                 | line_id | power              | origin_bus_id | ... |
----------------------------------------------------------------------------------
| 2019-01-01 21:00:00+00:00 | 1-4     | 195.21081151654957 | 1.0           | ... |
----------------------------------------------------------------------------------
```
- Filter the 864 returned events to those whose "Power _MVA" field equals the numeric literal
  - Notice how the `tonumber` function can go on the left-hand side of the `eval` expression
- Return the one matching event
  - Technically, the `tonumber` function is redundant in this example. The query works without it
## Get NULL field
```
sourcetype="scada" | eval zzz=tonumber('sourcetype')
```
- Return 864 events that do not have the field "zzz" because `tonumber` returns NULL for every event
# Purpose
- `tonumber` must be used with `eval`, `fieldformat`, `where`, or within an `eval` expression
- `tonumber` can parse a field value or a string literal
  - If a field value cannot be parsed to a number, the function returns NULL
    - A null field is a field that is omitted entirely from the result or event
  - If a string literal cannot be parsed to a number, it raises an error
- `tonumber` cannot be used to format an int as a float
  - Use `eval` for that
# Arguments
## Required
- A single field name or string literal
## Optional
- The base that the returned number should be in
  - This default to base-10