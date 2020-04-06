- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Managesearch-timefieldextractions - great introduction to all aspects of field
  extractions
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Createandmaintainsearch-timefieldextractionsthroughconfigurationfiles - great table
  comparing when to use different field extraction types (unused source at the moment)
# How to add search-time field extractions
- There are 3 ways to add search-time field extractions:
  - Add with the "Field Extractor" Splunk Web tool 
    - This is the easiest and fastest method
  - Add with the "Field extractions" page in Splunk Web
    - This is good for viewing field extractions, but not as good as the Field Extractor for creating them
  - Directly edit `props.conf`
    - See `3-edit-config-files` notes
- Field extractions created through the Field Extractor or the "Field extractions" page are only available to their creators until shared with others
  via new permissions
# Field extraction types
- There are two types of (search-time) field extractions: inline and transform
## Inline field extractions
- An inline field extraction is also referred to as a "Inline" type field extraction
- It is set up entirely within `props.conf` 
  - It always has a stanza of the form `EXTRACT-<extraction name>` 
- It is always powered by a field-extracting regular expression 
  - I presume it's always a single regular expression, but IDK
- An inline field extraction can apply to *multiple* \<specs> by using a regex as the \<spec> of the stanza (see source)
  - Technically, the documentation only says that a sourcetype can be a regex (not host or source), but we'll see
### Example
#### `$SPLUNK_HOME/etc/users/austin/launcher/local/props.conf`
```
[csv]
EXTRACT-timestamp,line_id,current,origin_voltage,destination_voltage = ^(?P<timestamp>[^,]+),(?P<line_id>[^,]+),(?P<current>[^,]+),(?P<origin_voltage>[^,]+),(?P<destination_voltage>.+)
```
- This is what it looks like when I use a regex instead of delimiter-based field extraction for a CSV (gross)
## Transform field extractions
- A transform field extraction is also referred to as a "uses transform" type field extraction
- It has components both in `props.conf` and `transforms.conf` 
  - The stanza for defining this type of extraction goes into `transforms.conf`
    - It always has a stanza of the form `[REPORT-<extraction name>]` 
  - Whatever sourcetype in `props.conf` that uses this extraction references it by its stanza name
- It is powered by either a field-extracting regular expression or delimiter(s) + fields
  - I presume if a regular expression is used, there is only one, but IDK
  - A delimiter-based field extraction is always a transform field extraction
- A transform field extraction can reference multiple field transforms if I want to apply more than one field-extracting regex to the same source,
  sourcetype, or host (see source)
  - E.g. if machine logs and csv data both provide data for the "price" field, I'll need multiple field-extracting regexs to make Splunk understand
    that these differente event patterns are providing the same data
### Example
#### `$SPLUNK_HOME/etc/apps/search/local/props.conf`
```
[csv]
REPORT-scada4 = REPORT-scada4
```
- One half of a transform field extraction
#### `$SPLUNK_HOME/etc/apps/search/local/transforms.conf`
```
[REPORT-scada4]
DELIMS = ","
FIELDS = "field1","field2","field3","field4","field5"
```
- The other half