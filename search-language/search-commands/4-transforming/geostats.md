- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2001/SearchReference/Geostats
# Background
- I don't know that the `geostats` command is transforming. I'm just guessing
- Using the `BY` clause appears to be forbidden
  - If it were allowed, it would enable me to do something like "count how many times substation 5 was with a geographic bin and count how many times
    substation 3 was within the same geographic bin", as opposed to counting every substation within that geographic bin together
# Arguments
## Required
- A statistical function
  - When asking myself which statistical function to use, just think "what statistical operation do I want to apply to each set of events within a
    geographic bin?" The `geostats` function just bins events based on their latitutde and longitude values
# Example
```
sourcetype="oms" 
| makemv delim="-" "Bus Affected" 
| mvexpand "Bus Affected" 
| lookup bus-locations-lookup-def bus_id AS "Bus Affected" OUTPUTNEW latitude longitude 
| geostats latfield=latitude longfield=longitude count
```
- Within each geographic bin, count the number of events within each bin