- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2003/Knowledge/Abouttagsandaliases
# Main example
## File
### `/Applications/Splunk/etc/apps/untopchan/local/tags.conf`
```
["Origin Voltage _PU"=1.0]
generation_voltage = enabled

[origin_bus_id=1]
generation_bus = enabled
northern_bus = enabled

[origin_bus_id=2]
generation_bus = enabled

[origin_bus_id=3]
generation_bus = enabled
```
## Searches
- `tag=generation_bus`
- `tag=generation_voltage`
- `tag=northern_bus`
# Purpose
- Tags provide convenient, flexible event grouping
  - E.g. the "origin_bus_id" field has multiple values that are related because those values are generation buses
- Tags provide convenient, flexible event metadata
  - E.g. if it ever mattered, I could search how buses were geographically located relative to one another. I can see that bus 1 is in the group of
    buses in the north 
- Tags can provide an illusion that data schemas stay consistent when they actually change
  - E.g. if the IP address of the main office changed, then I could tag the new IP address with the "main_office" tag and searches for
    `tag=main_office` would still work as expected
# Search evaluation workflow
- When a field-value pair is saved as a tag, searching for that tag will return all events that have that field-value pair
- Splunk does not validate tags at all
  - If the field-value pair doesn't exist (or there's a typo), Splunk doesn't care. It will return zero results
- Tags are the last knowledge object evaluated in a search
# Management workflow
- Splunk does not delete old field-value pairs
  - If I make a tag with an invalid field-value pair and then delete it, Splunk keeps that stanza around (but doesn't use it for anything)
- Use Splunk Web to manage tags
# Syntax
- Tag names must contain conventional Splunk characters: [a-z], [A-Z], [0-9], "_"
- A field-value pair can have one or more tags
- The same tag can be associated with one or more field-value pairs
- Make sure to use double quotes for fields when needed
- Tag searching is case-sensitive
# Files
- Tags are stored within `tags.conf`
- Which `tags.conf` is used depends on the permissions of the tag, like any other Splunk knowledge object
# Performance
- Splunk does not mention any heavy performance penalties for using tags