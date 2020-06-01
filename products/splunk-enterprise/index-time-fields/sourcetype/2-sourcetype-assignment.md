- https://docs.splunk.com/Documentation/Splunk/8.0.3/Admin/Inputsconf#inputs.conf.example - `inputs.conf` example
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Admin/Propsconf#props.conf.example - `props.conf` example
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Data/Advancedsourcetypeoverrides - per-event `sourcetype` overrides (example 2)
# Examples
- See the sources for detailed examples
## Blunt sourcetype assignment with `inputs.conf`
### File
#### `/Applications/Splunk/etc/apps/untopchan2/local/inputs.conf`
```
[script://$SPLUNK_HOME/etc/apps/untopchan2/bin/run_untopchan.sh]
disabled = 1
index = electrical_utility_data
interval = 90
sourcetype = realtime_scada
```
- `inputs.conf` must set the `index` and `source` attributes of events
  - If the `index` isn't explicitly set, it default to "main" or whatever other index I've chosen to be the default
    - The `defaultDatabase` attribute in `indexes.conf` is what actually sets the default index that is used
  - If the `source` isn't explicitly set, it defaults to the input path
    - In this example, source defaults to "/Applications/Splunk/etc/apps/untopchan2/bin/run_untopchan.sh"
  - Neither `props.conf` nor `transforms.conf` can set these attributes
- The `sourcetype` attribute usually must be set within `inputs.conf`, but not always
  - If I choose to set `sourcetype` here, all events from the data input should have same source type
    - If events from the same data input can have different source types, then I shouldn't set `sourcetype` here
  - The `sourcetype` can also be set in `props.conf`
## Fine-grained per-event sourcetype assignment with `inputs.conf`, `props.conf`, and `transforms.conf`
### Files
#### `/Applications/Splunk/etc/apps/untopchan2/local/inputs.conf`
```
[script://$SPLUNK_HOME/etc/apps/untopchan2/bin/run_untopchan.sh]
disabled = 1
index = electrical_utility_data
interval = 90
source = run_untopchan.sh
```
#### `/Applications/Splunk/etc/apps/untopchan2/local/props.conf`
```
[source::run_untopchan.sh]
TRANSFORMS-realtime_scada_sourcetype = match_realtime_scada_sourcetype
TRANSFORMS-realtime_oms_sourcetype = match_realtime_oms_sourcetype
```
#### `/Applications/Splunk/etc/apps/untopchan2/local/transforms.conf`
```
[match_realtime_scada_sourcetype]
REGEX = ^([^,]*,){4}[^,]*$
FORMAT = sourcetype::realtime_scada
DEST_KEY = MetaData:Sourcetype

[match_realtime_oms_sourcetype]
REGEX = ^([^,]*,){5}[^,]*$
FORMAT = sourcetype::realtime_oms
DEST_KEY = MetaData:Sourcetype
```
- First, optionally set the `source` attribute in `inputs.conf` so it has a nicer format
- Second, reference that source in `props.conf` with a stanza that contains transforms
- Third, configure stanzas in `transforms.conf` with the exact syntax as shown
  - See third source for the exact syntax
- This technique is complicated, but it is one flexible way of overriding the default input-based `inputs.conf` source type assignment
- This technique applies to the following scenarios:
  - A script outputs events with different source types
# Troubleshooting
## File-based input source type override assignment with `inputs.conf` and `props.conf`
### Files
#### `/Applications/Splunk/etc/apps/untopchan2/local/inputs.conf`
```
[script://$SPLUNK_HOME/etc/apps/untopchan2/bin/run_untopchan.sh]
disabled = 1
index = electrical_utility_data
interval = 90
source = run_untopchan.sh
```
#### `/Applications/Splunk/etc/apps/untopchan2/local/props.conf`
```
[source::run_untopchan.sh]
sourcetype = anaconda 
```
- `props.conf` specifically says that this technique only works for _file based inputs_. If I try to use this technique on a script or something else,
  this won't work. The events will have the default source type value: "exec"
# Useful attributes
- `pulldown_type = true` must be set within a source type stanza for the source type to be editable in Splunk Web