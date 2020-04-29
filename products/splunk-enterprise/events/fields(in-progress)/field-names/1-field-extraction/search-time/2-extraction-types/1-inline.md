- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Managesearch-timefieldextractions - mentions using regular expression with _only_ sourcetypes
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Configureinlineextractions - specific syntax for inline field extractions
# Extraction mechanism
- An `EXTRACT-<class>` attribute within a `props.conf` stanza defines a single regular expression that uses named capture groups to extract one or
  more fields from data that matches the given \<spec>
- Multiple `EXTRACT-<class>` attributes can exist within the same stanza
# When to use
- Use when a regular expression is the best way to correctly extract the fields
- Use when wildcard matching in a stanza is sufficient to apply the regular expression to multiple \<specs>, if desired
- Delimter-based field extraction cannot be done with inline field extractions. I must use a transform field extraction
# Files
- An inline field extraction is defined entirely with `props.conf`
- It can be created within Splunk Web (preferred) or by directly editing `props.conf`
- Make sure to restart Splunk to apply changes
# Syntax
- An inline field extraction is composed of an attribute of the form `EXTRACT-<class>` 
  - Multiple inline field extractions can exist within a \<spec>
- The inline field extraction(s) within a \<spec> can apply to "multiple" \<specs> by using a regex within the \<spec> (see source)
  - Technically, the documentation only says that a sourcetype can be a regex (not host or source), but we'll see
# Examples
## Multi-field inline field extraction
### `$SPLUNK_HOME/etc/users/austin/launcher/local/props.conf`
```
[csv]
EXTRACT-timestamp,line_id,current,origin_voltage,destination_voltage = ^(?P<timestamp>[^,]+),(?P<line_id>[^,]+),(?P<current>[^,]+),(?P<origin_voltage>[^,]+),(?P<destination_voltage>.+)
```
- Use an inline field extraction (instead of delimiter-based transform field extraction) for the csv sourcetype 
  - This field extraction layers on top of the built-in csv sourcetype
- The name of the field extraction is literally "EXTRACT-timestamp,line_id,current,origin_voltage,destination_voltage"