- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/SearchReference/MultivalueEvalFunctions#split.28X.2C.22Y.22.29
# Examples
- `sourcetype="oms" | eval buses=split('Bus Affected', "-")`
```
----------------------------------------------------------------------------------------------------------------------
| Event Datetime            | Restoration Datetime      | Line ID | Type         | Bus Affected | Amount _MW | buses |
----------------------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | 5-6     | line_outage  | 5-6          | 1.0        | 5     |
|                           |                           |         |              |              |            | 6     |
----------------------------------------------------------------------------------------------------------------------
| 2019-01-01 19:15:00+00:00 | 2019-01-01 19:30:00+00:00 | 6-7     | loss_of_load | 6            | 1.0        | 6     |
----------------------------------------------------------------------------------------------------------------------
| 2019-01-01 13:45:00+00:00 | 2019-01-01 14:00:00+00:00 | 7-8     | loss_of_gen  | 7            | 1.0        | 7     | 
----------------------------------------------------------------------------------------------------------------------
```
- For each of the three events, split the "Bus Affected" field into a multivalue field called "buses"
  - If the field cannot be split, leave the field as a uni-value field
- Return the three modified events
# Background
- `split` splits a uni-value field into a multivalue field based on the provided delimiter
# Arguments
## Required
- A field and a delimiter