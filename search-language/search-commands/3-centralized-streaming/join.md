- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/SearchReference/Join
- https://answers.splunk.com/answers/568034/chart-over-multiple-fields.html
# Examples
## Join subsearches as line ratings
```
sourcetype="scada" 
| timechart span=15m first("Power _MVA") BY "Line ID"
| join _time 
    [ search sourcetype="scada" 
    | eval "Power _MVA"=150 
    | timechart span=15m first("Power _MVA") as "5-6,6-7 line rating"] 
| join _time 
    [ search sourcetype="scada" 
    | eval "Power _MVA"=300 
    | timechart span=15m first("Power _MVA") as "3-6 line rating"] 
| join _time
    [ search sourcetype="scada" 
    | eval "Power _MVA"=250 
    | timechart span=15m first("Power _MVA") as "1-4,4-5,7-8,2-8,8-9,4-9 line rating"]
```
- Each subsearch gets all 864 events and replaces the "Power _MVA" field in each event with a dummy value, such as 300. Then, the subsearch is joined
  to the main search
  - This is a horrible command in terms of performance
- The result is that the `timechart` command charts multiple time series at the same time. Neat!
  - Is there a more performant way to achieve the same result? Yes there is. Choose a single line ID. Every time that line ID appears in an event,
    that event delimits a logical Splunk transaction which consists of the measurements taken at the same instant from different buses. Make the
    "Power _MVA" field of every targeted event into a multivalued field, then split the multivalued field into new events. Finally, change the "Line
    ID" of the new events into a line rating name depending on the "Power _MVA" field
    - See `mvexpand.md` notes
  - Even better, learn how to use Splunk dashboard inputs to allow users to toggle lines and their ratings on and off
# Background
- `join` is a centralized streaming command when a set of fields with which to join is provided
  - Otherwise, it is `dataset processing` command
- Yes, I *can* simply join multiple `timechart` results together, but using `BY` in the outer and inner searches does not work like I want
# Arguments
## Required
- A subsearch
## Optional
- A comma-separated list of fields upon which to join
- One or more \<join options>
