- https://docs.splunk.com/Documentation/SplunkCloud/latest/SearchReference/Mvexpand
# Examples
## Expand multivalue field into two new events
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
- For the 3 returned events, expand any event whose "buses" field is a multivalue field into however many new events it takes to assign each new event
  a single value from that multivalue field
  - The `split` function created the event with the multivalue field. That process isn't directly shown here
- The net result is that a total of four events are returned: two were created from a single event with a multivalue field while two were left alone
  because their "buses" field was a uni-value field
## Expand multivalue field into four new events
```
sourcetype="scada" 
| eval "Power _MVA"=if('Line ID'="1-4", mvappend('Power _MVA', "150r", "250r", "300r"), 'Power _MVA')
| mvexpand "Power _MVA"
| eval "Line ID"=case('Power _MVA'="150r", "5-6,6-7 line rating", 'Power _MVA'="250r", "1-4,4-5,7-8,2-8,8-9,4-9 line rating", 'Power _MVA'="300r", "3-6 line rating", 1=1, 'Line ID')
| eval "Power _MVA"=case('Power _MVA'="150r", 150, 'Power _MVA'="250r", 250, 'Power _MVA'="300r", 300, 1=1, 'Power _MVA')
| timechart span=15m limit=0 first("Power _MVA") BY "Line ID"
```
- Most of the work is being done with `eval` here, but `mvexpand` plays an important role in generating 3 new fake events to represent the line
  ratings at every measurement time interval
- This approach is preferred to using multiple statistical functions with `timechart` because the `by` clause is done for every statistical function!
# Purpose
- The `mvexpand` command only shows interesting results when a field is a multivalue field in at least one event
# Arguments
## Required
- A single field name that *can* be a multivalue field