- https://docs.splunk.com/Documentation/SplunkCloud/7.2.6/SearchReference/Dedup
# Background
- Removes events that contain identical values for the fields that I specify
- `dedup` can work with multiple fields, but not in a way that is complex enough for my needs
  - E.g. `... | dedup 2 source host` will keep the first two events that have the *same* values for their source and host fields. However, there is no
    way to do something like `... | dedup ((origin_bus_id, "Origin Voltage _PU"), (destination_bus_id, Destination Voltage _PU))` where two field
    pairs are compared within themselves *and* against other field pairs to remove duplicates across the two distinct pairs!
      - I'll have to reduce the two field pairs into a single field pair, which is easy enough
# Arguments
- x
# Examples
## Reduce two field pairs to a single field pair and dedup on the single field pair
```
sourcetype="scada" 
| eval o_pair='origin_bus_id' + "@@" + 'Origin Voltage _PU' 
| eval d_pair='destination_bus_id' + "@@" + 'Destination Voltage _PU' 
| eval od_pair=mvappend(o_pair, d_pair) 
| mvexpand od_pair
| dedup od_pair
| rex field="od_pair" "(?<unique_bus>\d*)@@(?<unique_voltage>\d.*)" 
| sort _time "Line ID"
```
- The "o_pair" field reduces two fields into a logical key
- The "d_pair" field reduces two fields into a logical key
- The "od_pair" field combines the keys into a single, multivalue field
- The `mvexpand` command makes every event expand into two events, where each event gets one of the values from the original "od_pair" multivalue
  field
- Now we have a single field that has every bus and every voltage as a key
- Use `dedup` to remove duplicate (\<bus_id>@@\<voltage>) keys
- Use `rex` to expand the "od_pair" field (which now contains unique (\<bus_id>@@\<voltage>) keys across both the origin voltage and destination
  voltage!) into simple "bus" and "voltage" fields
- Use the "bus" and "voltage" fields to do stuff!
  - Note that this command does *not* work across all time because it deletes events that have an existing od_pair, even if that od_pair is from a
    completely different time! A bus *can* have the same voltage at different times, just not within the same measurement time!
    - To get around this, just dedup with time: `... | dedup Datetime od_pair`
## First example again, but factor in time via timechart instead of dedup
```
sourcetype="scada" 
| eval o_pair='origin_bus_id' + "@@" + 'Origin Voltage _PU' 
| eval d_pair='destination_bus_id' + "@@" + 'Destination Voltage _PU' 
| eval od_pair=mvappend(o_pair, d_pair) 
| mvexpand od_pair
| rex field="od_pair" "(?<unique_bus>\d*)@@(?<unique_voltage>\d.*)"
| sort _time "Line ID"
| timechart span=15m first(unique_voltage) by unique_bus
```
- This shows the unique reading of each bus at every 15 minute measurement interval
- I think using `dedup` with time makes way more sense