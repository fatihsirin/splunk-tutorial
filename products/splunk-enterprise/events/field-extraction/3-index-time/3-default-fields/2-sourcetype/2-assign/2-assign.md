- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Whysourcetypesmatter - this is the introduction page, but towards the bottom it talks about
  the process of sourcetype assignment
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Bypassautomaticsourcetypeassignment#Specify_source_type_for_an_input - workflow for using
  Splunk Web to assign a sourcetype based on a data input
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Bypassautomaticsourcetypeassignment#Specify_source_type_for_a_source - workflow for directly
  editing `props.conf` to assign a sourcetype based on a data source (not a data input)
# Introduction
- Splunk searches though different files based on precedence to index incoming data with a particular sourcetype
- These steps and the corresponding files are listed below in descending precedence order:
  - Via data input with `inputs.conf` and/or `props.conf`
  - Via data source with `props.conf`
  - etc...
- If needed, review the `2-field-extractor-utility.md` notes to understand why files in certain directories are being modified
# Explicit sourcetype specification via data input
- This approach of assigning a sourcetype usually isn't recommened because it isn't granular
- When a data input is input into Splunk, I have the option to assign all future raw data from that input to be a specific sourcetype
  - These configuration changes may be saved in `inputs.conf`, `props.conf`, both, or neither
- These configuration changes can be done via Splunk Web when adding data, or by manually editing `inputs.conf` and/or `props.conf`
  - It's intuitive to use the Splunk Web interface, so I won't compare *how* the Splunk-Web vs. direct-edit workflows are different. It isn't
    necessary
## Modify `inputs.conf` and `props.conf` example
- If the data input being input is to be monitored continuously *and* the raw data is to be assigned to a new, custom sourcetype, then Splunk will
  modify `inputs.conf` and `props.conf`
- It doesn't matter if the continuous input is a file, directory, script, etc., what matters is that it's continuously monitored *and* that a new
  sourcetype was defined
### `/$SPLUNK_HOME/etc/apps/search/local/inputs.conf`
```
[monitor:///Users/austinchang/repositories/untopchan/dummy-csv.csv]
disabled = false
sourcetype = blah2_csv
```
- Splunk is being told to continuously monitor this CSV for changes, and to always assign any incoming raw data to be of the new "blah2_csv" soucetype
### `/$SPLUNK_HOME/etc/apps/search/local/props.conf`
```
[blah2_csv]
CHARSET = CSASCII
DATETIME_CONFIG = 
INDEXED_EXTRACTIONS = csv
KV_MODE = none
LINE_BREAKER = ([\r\n]+)
NO_BINARY_CHECK = true
SHOULD_LINEMERGE = false
category = Structured
description = Comma-separated value format. Set header and other settings in "Delimited Settings"
disabled = false
pulldown_type = true
```
- Since a new, custom soucetype was created, the soucetype definition was created in `props.conf`. If a default sourcetype was used instead, this file
  wouldn't have needed to be modified
## Modify only `inputs.conf` example
- If the data input being input is to be continuously monitored, but the assigned sourcetype already exists, then only `inputs.conf` needs to be
  modified
### `/$SPLUNK_HOME/etc/apps/search/local/inputs.conf`
```
[monitor:///Users/austinchang/repositories/untopchan/hist_rand_scada.csv]
disabled = false
sourcetype = csv
```
- Splunk ships with the default "csv" sourcetype, so `props.conf` never needs to be modified
## Modify only `props.conf` example
- If the data input being input is only indexed once, *and* the chosen sourcetype is a new, custom sourcetype, then only `props.conf` must be modified
  to define the new sourcetype. `inputs.conf` isn't modified since Splunk doesn't ever expect to get data from this input again
### `/$SPLUNK_HOME/etc/apps/search/local/props.conf`
```
[blah4_csv]
<configuration settings>
```
- Although I can't tell by looking at `props.conf` which file was indexed a single time by Splunk (maybe I can, but I don't know how), I can tell that
  when the file was indexed with the new, custom "blah4_csv" sourcetype, the definition of that sourcetype had to be created in `props.conf`
## Modify neither example
- If the data input being input is only indexed once and its assigned to an existing sourcetype, then neither `props.conf` nor `inputs.conf` will be modified
- No example is needed
# Explicit sourcetype specification via data source (bad)
- It doesn't appear that I can set the sourcetype though this method for any local input
  - Can I do it for a forwarded input?
- ... I don't really care. I need to come back to this
```
- I can override automatic sourcetype matching by modifying `props.conf` to explicitly assign a single sourcetype to all data from a specific source
- The documentation doesn't say that there is a way to make these modifications via Splunk Web, so it looks like I have to edit the configuration
  files directly
## Example
### `$SPLUNK_HOME/etc/system/local/props.conf`
- The changes in sourcetype assignment specified by this file won't go into affect unless ...
```
# Rule-based source type recognition
- x