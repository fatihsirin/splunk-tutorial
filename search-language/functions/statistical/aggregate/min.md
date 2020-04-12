# Background
- `min` returns the minimum value of a single field
  - If the field is numeric, it's the numeric minimum
  - If the field is non-numeric, it's the lexicographic minimum
- Actually, I can supply several comma-separated fields, but I don't get any results...
# Arguments
## Required
- A single field name
  - E.g. `... | min(<field name>)`
# Examples
## Get min power value and associated event
- `sourcetype="scada" AND power<=[search sourcetype="scada" | stats min(power) as search]`
```
-------------------------------------------------------------------------------------------------------------------------------
| timestamp                 | line_id | origin_bus_id | origin_voltage     | destination_bus_id | destination_voltage | power |
-------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 5-6     | 5             | 0.8997259228885474 | 6                  | 0.9962956703966502  | 0.0   | 
-------------------------------------------------------------------------------------------------------------------------------
```
- The subsearch calculates the minimum value of the "power" field across all events. It then returns that value in a special way, by renaming the
  "min(power)" field to be the "search" field. This triggers the special subsearch behavior that returns the value of the "search" field
  (0.0000000000000000) instead of returning the field-value pair (search=0.0000000000000000). The outer query gets the value 0.0000000000000000 and
  uses that value to compare against the values of "power" field within the 864 events that the outer search found. The use of the "<=" operator is
  required to trigger a numeric comparison. "<" would exclude all events (no event has a power values strictly less than the minimum) while "=" would
  perform a lexicographic comparison (the actual event contains 0.0, and "0.0" != "0.0000000000000000")
  - The "AND" is redundant but is used for clarity in this example
- Return the single matching event. Whew!