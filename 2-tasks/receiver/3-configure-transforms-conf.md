# Examples
## Assign sourcetype based on regex matching from same source
### Files
#### /opt/splunk/etc/apps/untopchan/default/transforms.conf
```
[match_realtime_scada_sourcetype]
REGEX = ^\d([^,]*,){4}[^,]*$
FORMAT = sourcetype::realtime_scada
DEST_KEY = MetaData:Sourcetype

[match_realtime_oms_sourcetype]
REGEX = ^\d([^,]*,){5}[^,]*$
FORMAT = sourcetype::realtime_oms
DEST_KEY = MetaData:Sourcetype

[REPORT-extract_scada_base_fields]
DELIMS = ","
FIELDS = "Timestamp","Line_ID","Power_MVA","Origin_Voltage_PU","Destination_Voltage_PU"

[REPORT-extract_oms_base_fields]
DELIMS = ","
FIELDS = "Event_Timestamp","Restoration_Timestamp","Line_ID","Type","Bus_Affected","Amount_MW"
```
- The `[REPORT-*` stanzas perform search-time field extraction
- The `[match_*]` stanzas could be named anything, not just "match_*", and they perform sourcetype assignment based on regex matching
  - A properly configured `props.conf` must be used with `transforms.conf` to do this