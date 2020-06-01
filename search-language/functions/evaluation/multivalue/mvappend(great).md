- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/MultivalueEvalFunctions#mvappend.28X.2C....29
# Examples
- `sourcetype="scada" | eval pair=mvappend(origin_bus_id, 'Origin Voltage _PU') | sort _time "Line ID"`
```
-------------------------------------------------------------------------------------------------------------------------------------------------------
| Datetime                  | Line ID | Power _MVA         | origin_bus_id | Origin Voltage _PU | destination_bus_id | Destination Voltage _PU | pair |
-------------------------------------------------------------------------------------------------------------------------------------------------------
| 2019-01-01 00:00:00+00:00 | 1-4     | 47.431454234640256 | 1             | 1.0                | 4                  | 0.9834063111345333      | 1    |
|                           |         |                    |               |                    |                    |                         | 1.0  |
------------------------------------------------------------------------------------------------------------------------------------------------------|
| <8 more rows>                                                                                                                                       |
-------------------------------------------------------------------------------------------------------------------------------------------------------
```
- For the 9 returned events, create a new field named "pair", where that field contains multiple values that were pulled from the "origin_bus_id"
  field and the "Origin Voltage _PU" field
- Return the 9 events with the new multivalue field
# Purpose
- The `mvappend` function _creates_ a new multivalue field by appending all of its arguments to a single field
# Arguments
## Required
- At least one uni-value field name, multi-value field name, or string
  - Numbers are invalid arguments