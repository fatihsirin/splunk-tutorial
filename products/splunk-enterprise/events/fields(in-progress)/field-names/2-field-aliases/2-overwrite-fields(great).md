- https://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Addaliasestofields
# Main examples
## First example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::hist_matpower_scada.csv]
FIELDALIAS-badAlias = blah AS origin_bus_id
```
- "Overwrite field values" was checked
### Searches
- `sourcetype="scada"`
## Second example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::hist_matpower_scada.csv]
FIELDALIAS-badAlias = "Line ID" AS origin_bus_id
```
- "Overwrite field values" was checked
### Searches
- `sourcetype="scada"`
# Purpose
- If "Overwrite field values" is checked in Splunk Web, Splunk evaluates aliases differently when conflicts arise
# Search evaluation workflow
- In the first example with "badAlias", all real values of the real field "origin_bus_id" are set to NULL because the real field "origin_bus_id"
  doesn't exist anymore. The real field was replaced by the alias "origin_bus_id". Since the field "blah" doesn't exist in the data, its alias cannot
  have any values.
  - The end result is that real "origin_bus_id" values are lost because the field doesn't exist anymore
  - If "Overwrite field values" wasn't checked, then the real field "origin_bus_id" and its value would be left alone
- In the second example with "badAlias", all real values of the field "origin_bus_id" are replaced with values from the field "Line ID". This happens
  because override behavior was enabled, so the alias "origin_bus_id" (which is supposed to have the same values as "Line ID"), replaces the real
  field "origin_bus_id"
  - The end result is that real "origin_bus_id" values are lost because the field doesn't exist anymore
  - If "Overwrite field values" wasn't checked, then the real field "origin_bus_id" and its value would be left alone
# Management workflow
- Don't select this box because I can't think of a sitution where it would be beneficial
# Files
- I can't tell where the "Overwrite field values" data is stored. It isn't in `props.conf`