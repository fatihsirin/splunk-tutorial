- https://docs.splunk.com/Documentation/Splunk/8.0.3/SearchReference/Bin
# Background
- `bin` is called by the `chart` and `timechart` commands automatically
- Those two command are preferable to the `bin` command. Use it only when the other two commands aren't sufficient
- This command is distributable streaming if invoked with the `span` argument
  - This makes sense because bucketing by time can be done as events are retrieved from the index
# Arguments
## Required
- x
# Examples
## Histogram of the collective voltage readings of all buses
```
sourcetype="scada" 
| eval o_pair='origin_bus_id' + "@@" + 'Origin Voltage _PU' 
| eval d_pair='destination_bus_id' + "@@" + 'Destination Voltage _PU' 
| eval od_pair=mvappend(o_pair, d_pair) 
| mvexpand od_pair
| dedup Datetime od_pair
| rex field="od_pair" "(?<unique_bus>\d*)@@(?<unique_voltage>\d.*)" 
| bin unique_voltage bins=10
| stats count by unique_voltage
```
- This creates a histogram which counts how many times all of the buses *together* were within a certain voltage band
  - The results are not that interesting. Collectively, the buses fall into the 0.9 - 1.0 voltage band 774 times out of 864 events
- Since I'm assured of having a single, unique voltage reading for each bus at each measurement time, I first turn each unique_voltage reading into a
  `bin` interval, then count how many times that interval appears
## Histogram of the voltage readings of single bus
- `sourcetype="scada" | where 'Line ID'="1-4" | bin "Destination Voltage _PU" bins=10 | stats count by "Destination Voltage _PU"`