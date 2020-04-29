- https://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Addaliasestofields - introduction to field aliases
# Main example
## File
### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::hist_matpower_scada.csv]
FIELDALIAS-voltage!# @rename = "Destination Voltage _PU" ASNEW destination_voltage "Origin Voltage _PU" ASNEW origin_voltage
FIELDALIAS-another_voltage_rename = "Destination Voltage _PU" ASNEW DV "Origin Voltage _PU" ASNEW DV
```
## Searches
- `sourcetype="scada" destination_voltage>=1.0`
- `sourcetype="scada" origin_voltage=1.0`
- `sourcetype="scada" DV>=1.0`
# Purpose
- An alias of a field gives the user another way to search for a field
- An alias can be used to give an ugly field name a prettier name, without removing the original field name
- Aliases are stronger than tags and event types, so they really should only be used to rename field names, not create new relationships between
  events
# Search evaluation workflow
- Searching a field alias is case-sensitive, just like with a normal field
- A field alias and its original field both appear in the selected/interesting fields sidebar
  - See `2-overwrite-fields.md` for exceptions
# Management workflow
- When creating an alias for a field in Splunk Web, don't put double quotes around the original field name
- I cannot alias the same field twice within the same field alias, but I can alias the same field in separate field aliases
- If I directly edit `props.conf` to manage field aliases, restart Splunk Web to apply changes
  - Why is this necessary? I can only guess that making updates with Splunk Web applies the changes to the data structures within the running Splunk
    process. Editing `props.conf` does NOT do that. Splunk Web must also therefore be reading from some in-memory data structures. It does not appear
    to parse `props.conf` every time I reload the web page
# Syntax
- Field alias names are not restricted to conventional Splunk character
  - They can have characters other than [a-z], [A-Z], [0-9], and "_"
- In a `FIELDALIAS-<class>` attribute, \<class> _is_ the name that was given to the field alias when it was created
- A single field can have multiple aliases
  - E.g. "Destination Voltage _PU" has "DV" and "destination_voltage" as aliases
- A single alias _should not_ apply to multiple fields, but Splunk doesn't validate aliases!
  - E.g. "DV" should not be applied to "Destination Voltage _PU" and "Origin Voltage _PU", but it is! Splunk chooses to have DV actually alias
    "Destination Voltage _PU", but never ever do this
# Files
- A field alias is defined within a host, source, or source type stanza in `props.conf`
# Performance
- The documentation says "field aliases for all source types are used in all searches, which can produce a lot of overhead over time"
  - Therefore, use aliases sparingly. I'm assuming this means that Splunk must search for any alias in every single search