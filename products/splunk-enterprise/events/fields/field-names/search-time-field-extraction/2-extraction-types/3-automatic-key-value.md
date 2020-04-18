- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Automatickey-valuefieldextractionsatsearch-time
# Extraction mechanism
- Automatic key-value extraction cannot be explicitly configured to extract specific fields
- Instead, the `KV_MODE` attribute is set to one of six values and the chosen value makes Splunk parse data with the associated `sourcetype`,
  `source`, or `host` \<spec> according to the rules associated with the value
  - E.g. `KV_MODE = json` will make Splunk interpret data as JSON and auto-magically extract keys and values
# When to use
- This type of field extraction _is_ what generates Splunk's "interesting fields" in the search results
  - Interesting fields are those fields that appear in at least 20% of the results returned by a search
- Splunk does not provide explicit situations where automatic key-value extraction is preferred to inline or transform field extractions
  - This type of extraction is more convenient than the other two. That's pretty much it's only winning trait
    - E.g. if some event had 100 JSON keys, I would never want to write explicit field extractions to convert every JSON key into a field
# Files
- Automatic key-value field extractions are written entirely within `props.conf`
- This type of field extraction cannot be created within Splunk Web
- Inline and transform field extractions say that a Splunk deployment must be restarted for them to take effect, so I'll assume the same for automatic
  key-value extractions
# Syntax
- See source for details
- `KV_MODE` can be:
  - none: disable automatic field extraction
    - This will increase search performance because unnecessary fields won't be extracted
    - Since this type of field extraction occurs after inline and transform field extractions, this value will prevent _these_ extractions from
      overriding earlier extractions
  - auto (default): find as many interesting fields as possible 
    - This is the default value is the `KV_MODE` attribute isn't included at all in a stanza
  - auto_escaped: ensure that Splunk recognizes the `"\` and `\\` escape sequences within quoted values
  - multi: extract field values from table-formatted events via the `multikv` distributable streaming command
  - xml
  - json: if using this value, don't also set `INDEXED_EXTRACTIONS = JSON` because the same fields will be extracted twice: once at search time and
    once at index time
  - other weird random values that I can't find documentation for
# Examples
## Mismatched KV_MODE
### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[circuitComponent]
KV_MODE = xml
```
- This is a custom sourcetype called "circuitComponent" 
- When this sourcetype is used with some JSON raw data, Splunk doesn't find any additional interesting fields
  - Splunk always seems to generate "index", "linecount", "punct", and "splunk_server" in addition to host, source and sourcetype. These must be
    either index-time field extractions or something else built-in
    - Splunk's interesting fields are not magic after all!
## Correct KV_MODE
### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[circuitComponent]
KV_MODE = json
```
- This is the same sourcetype, but modified
- When the same JSON raw data is supplied, Splunk generates a whole host of interesting fields that actually come from the raw data!
  - E.g. "rated_power", "state_of_charge", etc.
- When I change the `KV_MODE` back to "none", the fields disappear (verified)
- When I delete `KV_MODE` entirely, the sourcetype implicitly defaults to `KV_MODE = auto`, so the interesting fields reappear (verified)
