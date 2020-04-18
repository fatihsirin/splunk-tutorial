- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Createandmaintainsearch-timefieldextractionsthroughconfigurationfiles - transform field
  extraction use cases
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Configureadvancedextractionswithfieldtransforms - transform field extraction syntax
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Exampleconfigurationsusingfieldtransforms - apply multiple regular expressions example
# Extraction mechanism
- The extraction mechanism consists of configuration defined across two files: `transforms.conf` and `props.conf`
- A "field transform configuration" is defined in `transforms.conf` 
  - This configuration can be a regular expression or delimiter-based
    - If it's delimiter-based, various attributes combine to produce the desired behavior
  - A _single_ regular expression can be defined for _each_ transform configuration
    - In order to apply _multiple_ regular expressions to the same `props.conf` stanza, the names of the transform configurations in `transforms.conf`
      are _all_ listed after the `REPORT-<class>` attribute of the `props.conf` stanza
      - It is _not_ the case that multiple regular expressions can be defined under the same transform configuration in `transforms.conf`
  - I don't think it makes sense to have multiple delimiter-based extractions
    - How could you? The `DELIMS` and `FIELDS` keys already accept multiple values!
- A "field extraction configuration" is defined in `props.conf`
  - Multiple extraction configurations can share the same transform configuration
    - In other words, multiple stanzas in `props.conf` can reference the same transform configuration in `transforms.conf` by listing the unique name
      of the transform configuration in their respective `REPORT-<class>` attributes
- The documentation says that "Extraction configurations in props.conf are restricted to a specific source, source type, or host," but what about
  \<rule> or \<delayedrule>?
  - IDK right now
# When to use
- A transform field extraction is also referred to as a "uses transform" type field extraction
- Use this to apply the same regular expression to multiple \<specs>, especially when wildcard matching in the stanza wouldn't be sufficient for doing
  this
- Use this to apply multiple regular expressions to the same \<spec>
- Must be used for delimiter-based field extraction
- Must be used to configure extraction for multivalue fields 
- Use to extract fields whose names begin with numbers and underscores?
  - I hope I never see this
- Use to manage the formatting of extracted fields
# Files
- A transform field extraction is defined with `props.conf` and `transforms.conf`
- Make sure to restart Splunk to apply changes
# Syntax
- Each transform configuration within `transforms.conf` must have a unique name
  - The name is not required to follow typical field name syntax (but try to avoid doing this)
    - Characters outside of a-z, A-Z, 0-9, and "_" are allowed (verified)
## Schema
- These are the less understood attributes that can exist under a transform stanza in `transforms.conf`
### FORMAT (optional)
- This attribute can be omitted if there is a REGEX attribute with _named_ capture groups
- The `FORMAT` attribute allows me to define how extracting group numbers and/or string literals will be combined to form the final key-value pair for
  the event
  - E.g. `FORMAT = $1::$2` will use the first extracting group number as the field name and the second extracting group number as the field value
  - E.g. `FORMAT = foobar::$1` will always use "foobar" as the field name and the first capture group as the field value
- Note that while an (essentially) unlimited number of capture groups (and associated extracting group numbers) may be used, concantenated fields can
  only be created with `FORMAT` at _index time_
### Others
- See source and update these notes if needed. `FORMAT` was the hardest to understand
# Examples
## Delimiter-based extraction
- This configuraiton layers on top of the default configuration for the built-in `csv` sourcetype
### `$SPLUNK_HOME/etc/apps/search/local/props.conf`
```
[csv]
REPORT-scada4 = REPORT-scada4
```
- This is the field extraction configuration
### `$SPLUNK_HOME/etc/apps/search/local/transforms.conf`
```
[REPORT-scada4]
DELIMS = ","
FIELDS = "field1","field2","field3","field4","field5"
```
- This is the field transform configuration
- It just so happens that the field extraction name and the transform stanza are the same
## Apply multiple transform configurations to the same `props.conf` stanza
- See `1-introduction.md`. The next example also fits this description
## Use the `FORMAT` attribute
```
[method=GET] [IP=10.1.1.1] [headerName=Host] [headerValue=www.example.com], [headerName=User-Agent] [headerValue=Mozilla], [headerName=Connection] [headerValue=close] [byteCount=255]
```
- This is a literal content of a web server log message
```
[myplaintransform]
REGEX=\[(?!(?:headerName|headerValue))([^\s\=]+)\=([^\]]+)\]
FORMAT=$1::$2
```
- This is one of two needed transform configurations that will get _all_ of the key-value pairs from the log data into a single event
- The `REGEX` value matches `[method=GET]` and `[IP=10.1.1.1]` to form `method=GET` and `IP=10.1.1.1` for the event
  - This part of the regex, `(?!(?:headerName|headerValue))`, ensures that text like `[headerName=Host]` or `[headerValue=www.example.com]` won't
    be matched by _this_ regex. I want this because I don't want a literal key-value pair like `headerName=Host`
- There are no _named_ capture groups in the `REGEX`, so `FORMAT` must be used in each transform extraction to define how the extracting group
  numbers will be used to form a final key-value pair
```
[mytransform]
REGEX= \[headerName\=(\w+)\],\s\[headerValue=([^\]]+)\]
FORMAT= $1::$2
```
- This is the second needed transform configuration
- The `REGEX` value matches `[headerName=Host]` and `[headerValue=www.example.com]` to form `Host=www.example.com` for the event
  - While the "header*" strings are matched by the regex, only the text matched by the cature groups is extracted into an extracting group number
- There are no _named_ capture groups in the `REGEX`, so `FORMAT` must be used in each transform extraction to define how the extracting group
  numbers will be used to form a final key-value pair
