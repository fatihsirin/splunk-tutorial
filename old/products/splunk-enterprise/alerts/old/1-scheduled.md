- https://docs.splunk.com/Documentation/Splunk/latest/Alert/Definescheduledalerts
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Savedsearchesconf
# Main example
## File
### `$SPLUNK_HOME/etc/apps/untopchan/default/savedsearches.conf`
```
[Line Outage Alert]
alert.expires = 120s
alert.severity = 5
alert.suppress = 0
alert.track = 1
counttype = number of events
cron_schedule = * * * * *
description = This alert triggers when Power_MVA<=0.0
dispatch.earliest_time = 1588809600
dispatch.latest_time = 1588910399
display.events.fields = ["Event_Timestamp","Timestamp","Restoration_Timestamp","Line_ID","Power_MVA","Origin_Bus_ID","Origin_Voltage_PU","Datetime","Destination_Bus_ID","Event Datetime","Restoration Datetime","Line ID","Type","Bus Affected","line_id","Power _MVA","started_timestamp","origin_bus_id","restored_timestamp","Origin Voltage _PU","destination_bus_id","type","bus_affected","x","min_power","y","voltage_diff","zzz","amount","search","Destination Voltage _PU","Amount _MW","quack","all_buses","buses","'Line ID'","bus","pair","od_pair","unique_bus","unique_voltage","lat","lon","latitude","longitude","rating","1-4,4-5,7-8,2-8,8-9,4-9 line rating","3-6 line rating","5-6,6-7 line rating","DV","hist_line_id","bar_hello","foo_hello","baz_hello","boo_hello","Destination_Voltage_PU","Bus_Affected","Amount_MW","sourcetype"]
display.events.type = table
display.visualizations.charting.chart = line
enableSched = 1
quantity = 0
relation = greater than
request.ui_dispatch_app = untopchan
request.ui_dispatch_view = search
search = index=electrical_utility_data Power_MVA<=0.0
```
#### Alert attributes
- alert.track: whether or not to track this alert via the "Triggered Alerts" page in Splunk Web. The attribute can have several values:
  - auto: ?
  - true:
  - false: don't track the alert in Splunk Web


- alert.expires: how long an alert persists as a record on the "Triggered Alerts" page before the record is removed and no longer displayed 
- alert.suppress: whether or not alert suppression is enabled for the scheduled search

## Searches
-
# Purpose
-


# Best practices for scheduled alert creation
## Match the alert schedule and search time range
- A scheduled Splunk "alert" comprises three distinct components: an underlying search, an alert schedule, and a trigger action
  - The underlying search is a normal search with its own time range
  - The alert schedule is when/how often the underlying search is run
  - The trigger action (e.g. sending a POST request) only happens if the events returned by the underlying search meet some criteria (e.g. there were
    5 events)
- Usually, the search time range of the underlying search _should_ match the alert schedule interval
  - E.g. if the underlying search searches events in the past 1.5 hours, then the search should be run every 1.5 hours (i.e. the alert schedule
    should be 1.5 hours)
    - If the alert schedule is greater than the search time range, some events could be missed. There could be false negatives
      - E.g. if the alert schedule is every two hours, .5 hours worth of events will never be searched
    - If the alert schedule is less than the search time range, some events could be double-counted. There could be false positives
      - E.g. if the alert schedule is every hour, then .5 hours worth of events will be included in two of the underlying searches
## Schedule the underlying search with at least one minute of delay
- The time range of the underlying search should end one minute or more before the current time
  - E.g. if an alert runs at 3:30 pm, 4:30 pm, etc. it should search events between 2:29 pm - 3:29 pm, 3:29 pm - 4:29 pm, etc.
- The reason for this delay is to ensure that all incoming events have been stored in the index and are ready for searching
# Management workflow
- 
# Syntax
## Cron expression syntax
### Time component ranges
- Minute: [0, 59]
- Hour: [0, 23]
- Day of the month: [1, 31]
- Month: [1, 12]
- Day of the week [0, 6]
  - 0 corresponds to Sunday
### Examples
- `0,30 * * * *` run at minute 0 and minute 30 (i.e. every 30 minutes), every hour, every day of the month, every month, every day of the week
- `*/30 * * * *` run at every minute that is divisible by 30, every hour, every day of the month, every month, every day of the week
  - E.g. 0/30 != 1, so it won't run at the start of every hour
  - E.g. 30/30 == 1, so it will run every hour at the 30 minute mark (e.g. 1:30 pm, 2:30 pm, etc.)
  - E.g. 60/30 is not valid because the minute range is [0,59]
# Files
-
# Performance
-