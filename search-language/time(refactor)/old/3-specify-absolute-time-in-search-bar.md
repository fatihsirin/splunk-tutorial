- https://docs.splunk.com/Documentation/Splunk/latest/Search/Specifytimemodifiersinyoursearch#Specify_absolute_time_ranges
# Examples
## Search-bar time modifiers only apply to the entire base search or an individual subsearch
- The following searches return different results
  - `index=electrical_utility_data sourcetype=realtime_scada AND Power_MVA<=[search index=electrical_utility_data sourcetype=realtime_scada | stats min(Power_MVA) as search]`
  - ``

## It is possible to search based on index time instead of the default event type
- x
# Purpose
- Absolute and relative time modifiers behave identically. They just have different syntax
# Syntax
- Remember that absolute times are specified in local time
- A snap-to specifier cannot be used with an absolute time
- Relative and absolute times can be used together

Get the minimum power at 5/12/2020 23:34:00
`index=electrical_utility_data sourcetype=realtime_scada earliest=5/12/2020:23:45:00 latest=5/12/2020:23:45:00 | stats min(Power_MVA)`