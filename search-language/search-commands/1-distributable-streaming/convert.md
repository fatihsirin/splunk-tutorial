- https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Convert#Convert_functions
# Examples
## Search for a numeric value
```
`sourcetype="scada" | convert num("Power _MVA") | search "Power _MVA"=195.21081151654957000000`
```
```
----------------------------------------------------------------------------------
| timestamp                 | line_id | power              | origin_bus_id | ... |
----------------------------------------------------------------------------------
| 2019-01-01 21:00:00+00:00 | 1-4     | 195.21081151654957 | 1.0           | ... |
----------------------------------------------------------------------------------
```
- For the 864 returned events returned by the first search, convert every value in the "Power _MVA" field into a number, then run a search over the
  864 returned events to find the event(s) whose "Power _MVA" field is equal to 195.21081151654957000000
  - Notice how even though the search numeric literal does not match the stored literal, the event is still found because a numeric comparison is
    used. The second search does not work on its own because Splunk _always_ does string comparisons with the = and != operators, and there is no way
    to change this
- Return the one event that was found by the second search
## Use multiple sub-functions
```
`sourcetype="scada" line_id="4-9" | convert auto(origin_voltage) auto(destination_voltage) | eval voltage_diff=(origin_voltage - destination_voltage) | sort _time`
```
```
----------------------------------------------------------------------------------
| timestamp                 | line_id | power              | origin_bus_id | ... | 
----------------------------------------------------------------------------------
| 2019-01-01 21:00:00+00:00 | 1-4     | 195.21081151654957 | 1.0           | ... |
| <95 more rows>                                                                 |
----------------------------------------------------------------------------------
```
- The `convert` function isn't necessary for the `eval` function to work properly, but this example does show how to use multiple sub-functions and
  thus how to convert multiple fields
# Purpose
- The `convert` function converts the values of a single specified field into numerical values. *How* it does this is dependent on whichever function
  is invoked
- `convert` cannot be used to format an int as a float
  - Use `eval` for that
# Arguments
## Required
- One or more sub-functions:
  - `auto`: for the specified field of every returned event, attempt to convert the field value into a number. If the value cannot be converted, leave
    the entire field value unchanged
  - `num`: for the specified field of every returned event, if the field values is a valid number, convert the string value into a number, else
    replace the string with the empty string
- One field name per sub-function