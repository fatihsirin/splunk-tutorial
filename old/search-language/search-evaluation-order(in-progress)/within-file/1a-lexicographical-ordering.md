- https://docs.splunk.com/Documentation/Splunk/8.0.3/Admin/Attributeprecedencewithinafile
# Main example
## File
### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::...hist...]
FIELDALIAS-formatting = "Line ID" ASNEW hist_line_id

[source::...matpower...]
FIELDALIAS-formatting = "Line ID" ASNEW matpower_line_id

[source::...atpower...]
FIELDALIAS-formatting = "Line ID" ASNEW atpower_line_id
```
## Searches
- `sourcetype="scada"`
# Purpose
- This ordering only can apply when two or more stanzas 1) affect the same \<spec> and 2) define the same attribute 
# Search evaluation workflow
- When two or more stanzas that affect the same \<spec> also define the same attribute, the stanza with the lowest lexicographical ordering takes
  precedence
  - E.g. the only alias that exists is "atpower_line_id", even though `FIELDALIAS-formatting` is defined in three stanzas
    - "a" comes before "h" which comes before "m"
# Management workflow
- Try not to ever do this
# Syntax
- Splunk doesn't consider the "..." pattern to be a regular expression. Therefore, stanzas with "..." are literal-matching stanazas
# Files
- This ordering only can apply within a single `props.conf` file