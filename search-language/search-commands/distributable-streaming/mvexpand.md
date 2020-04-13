- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/SearchReference/Mvexpand
# Background
- `mvexpand` only shows interesting results when a field is a multivalue field in at least one event
# Arguments
## Required
- A single field name that *can* be a multivalue field
# Examples
- `sourcetype="oms" | eval buses=split('Bus Affected', "-") | mvexpand buses`
```
----------------------------------------------------------------------------------------------------------------------
| Event Datetime            | Restoration Datetime      | Line ID | Type         | Bus Affected | Amount _MW | buses |
----------------------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | 5-6     | line_outage  | 5-6          | 1.0        | 5     | 
----------------------------------------------------------------------------------------------------------------------
| 2019-01-01 23:45:00+00:00 | 2019-01-02 00:00:00+00:00 | 5-6     | line_outage  | 5-6          | 1.0        | 6     |
----------------------------------------------------------------------------------------------------------------------
| <2 more rows>                                                                                                      |
----------------------------------------------------------------------------------------------------------------------
```
- For the 3 returned events, expand any event whose "buses" field is a multivalue field into however many new events it takes to make each event take
  a single value from that multivalue field. Remove the multivalue field event once it has been expanded into the new events
  - The `split` function created the multivalue event that isn't directly shown here
- The net result is that a total of four events are returned: two were created from a single event with a multivalue field while two were left alone
  because their "buses" field was a uni-value field