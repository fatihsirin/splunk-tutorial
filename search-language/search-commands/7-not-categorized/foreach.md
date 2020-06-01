- https://docs.splunk.com/Documentation/SplunkCloud/latest/SearchReference/Foreach
# Examples
## Calculate voltage difference
```
index=electrical_utility_data sourcetype=realtime_scada 
| eval Voltage_difference = 'Origin_Voltage_PU' 
| foreach Destination_Voltage_PU 
    [ eval Voltage_difference = Voltage_difference - '<<FIELD>>']
```
- This command calculates the voltage difference between the Origin_Voltage_PU and the Destination_Voltage_PU
  - The first `eval` command is critical. The "Voltage_difference" field won't exist in the final results unless this command is run
- The example would be much more interesting if there were a 100 different voltage readings in each event, and I wanted to do an operation across all
  of them
# Purpose
- The `foreach` command must be piped into. It operates separately on each individual event returned by the base search. For each individual event, it
  runs a subsearch on the fields of that individual event. It iterates over the specified fields in the event
- It appears to be a convenience command. I can't think of a situation where the same functionality couldn't be achieved with a plain `eval` command
# Arguments
## Required
- A list of one or more field names
  - Wildcards are allowed in the field names
- A subsearch that has the `'<<FIELD>>>'` template string that is a substitute for the value of each specified field of the single event