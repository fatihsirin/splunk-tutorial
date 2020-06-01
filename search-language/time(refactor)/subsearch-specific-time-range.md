# Examples
## Compare events across all time to a time-bounded minimum value
```
index=electrical_utility_data sourcetype=realtime_scada 
| eval min_power = 
    [ search index=electrical_utility_data sourcetype=realtime_scada earliest="5/12/2020:19:45:00" latest="5/12/2020:19:45:01" 
    | stats min(Power_MVA) as minimum_power | return $minimum_power] | where Power_MVA > min_power - 1 and Power_MVA < min_power + 1
```
- This search finds the minimum "Power_MVA" between the specified time range, as opposed to across the entire time range. Then, the "Power_MVA" field
  of all realtime_scada events is compared against that minimum value
  - This is very powerful and could not be achieved by using the time range picker alone
- This demonstrates that when time modifiers are used within a subsearch, that time range only applies to the subsearch
# Purpose
- Providing an explicit time range in the search bar can enable powerful analysis that wouldn't otherwise be possible