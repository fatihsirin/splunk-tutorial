# Examples
## Assign sourcetype based on regex matching from same source
### Files
#### /opt/splunk/etc/apps/untopchan/default/props.conf
```
#[source::run_untopchan.sh]
[source::call-python.sh]
TRANSFORMS-realtime_scada_sourcetype = match_realtime_scada_sourcetype
TRANSFORMS-realtime_oms_sourcetype = match_realtime_oms_sourcetype

[realtime_scada]
REPORT-scada_base_fields = REPORT-extract_scada_base_fields
EXTRACT-Origin_Bus_ID,Destination_Bus_ID = ^[^,\n]*,(?P<Origin_Bus_ID>\d+)[^\-\n]*\-(?P<Destination_Bus_ID>\d+)
category = Custom
pulldown_type = true

[realtime_oms]
REPORT-oms_base_fields = REPORT-extract_oms_base_fields
category = Custom
pulldown_type = true
```
# Purpose
- This is an example of assigning the sourcetype of an event based on regex matching
- Events from the _same_ source can have _different_ sourcetypes doing this kind of configuration
  - A properly configured `transforms.conf` is also required
  - It does not matter whether events were collected on the indexer or are from a forwarder, so long as they have the matching value for their
    `source` attribute