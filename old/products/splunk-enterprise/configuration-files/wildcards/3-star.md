- https://docs.splunk.com/Documentation/Splunk/8.0.3/Data/Specifyinputpathswithwildcards
# Examples
## Match example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::*matpower_*]
FIELDALIAS-test = "Line ID" ASNEW star_line_id
```
### Searches
- `sourcetype="scada"`
## Literal non-match example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::*fatpower_*]
FIELDALIAS-test = "Line ID" ASNEW whoo
```
### Searches
- `sourcetype="scada"`
## Path non-match example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::/Users/austinchang/repositories/untopchan/foo_bar.json]
FIELDALIAS-foo = hello ASNEW foo_hello

[source::/Users/*/repositories/untopchan/foo_bar.json]
FIELDALIAS-bar = hello ASNEW bar_hello

[source::/Users/*/untopchan/foo_bar.json]
FIELDALIAS-baz = hello ASNEW baz_hello
```
### Searches
- `sourcetype="circuitComponent"`
# Purpose
- The star `*` wildcard character matches anything but the path separator zero or more times
  - In the match example, the stanza matches the source "hist_matpower_scada.csv" because that entire source name is itself a single path segment
- The star is greedy, but Splunk somehow makes it work so that literal characters paired with the wildcard also have to match
  - In the literal non-match example, the stanza doesn't match the source name "hist_matpower_scada.csv" because "fatpower" isn't a substring of that
    name
- `*` behaves identically to `...`, except that `*` does not match path separator characters while `...` does
  - The alias in the last stanza in the path non-match example isn't applied for that reason
# Files
- Source and host stanzas (where `*` can be used) are defined within `props.conf`
- `*` can also be used in input stanzas with `inputs.conf`