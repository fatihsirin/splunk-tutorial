- https://docs.splunk.com/Documentation/SplunkCloud/latest/Knowledge/Searchtimeoperationssequence - order of search time operations
# Search workflow order summary
1. Inline field extractions
  - These are the `EXTRACT-<name>` field extractions in a `props.conf` stanza
  - Fields that are extracted by an inline field extraction whose name has an earlier lexicographical sort position _can_ be referenced inside inline
    field extractions having a name with a later lexicographical sort position
      - E.g. fields extracted by `EXTRACT-aaa` can be referenced _within_ `EXTRACT-zzz`, but not vice versa
2. Transform field extractions
  - These are the `REPORT-<name>` field extractions defined in `transforms.conf` and referenced in `props.conf`
3. Automatic key-value extraction
  - These are stanzas in `props.conf` that must be manually entered
4. 











- https://docs.splunk.com/Documentation/Splunk/6.1.4/Search/Aboutsearchlanguagesyntax#Fields - all fields are strings
# Splunk fields only contain strings
- Each value of a field is a text string
  - Numbers are *strings* that only contain numeric literals
    - E.g. the field containing the value of the number 10 contains the characters "1" and "0"
  - Commands that take numbers from string values automatically convert the values *internally* to numbers for calculations
## Repercussions
### Searching for numeric values is difficult
- Since all Splunk field values are strings, searching for events that contain a specific numeric values is extremely tricky
  - E.g. `sourcetype="scada" "Power _MVA"=195.21081151654957` returns one event as expected while `sourcetype="scada" "Power
    _MVA"=195.2108115165495700` returns zero events. Why? Because while Spunk *is* searching on the real "Power _MVA" field, the `search` command only
    does string comparisons with the = operator, so only the first query exactly matches the string value stored within an event. The second query has
    two extra 0s
#### Solutions
- See the `convert` function
- Using a `trim` function is not the solution because I never know how a numeric value will be stored in _raw