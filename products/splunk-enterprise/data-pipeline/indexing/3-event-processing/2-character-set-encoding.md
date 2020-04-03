- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Overviewofeventprocessing - high-level introduction
# Event processing stages
- When Splunk Enterprise indexes events, it performs "event processing"
    - This is also known as the parsing stage of the data pipeline
- Event processing consists of numerous stages which are ordered in the notes
# Configure the character set encoding
- Splunk attempts to apply UTF-8 encoding to sources by default (i.e. translate it as if it were already encoded in UTF-8)
- If a source does not use a UTF-8 encoding, or it is a non-ASCII file, Splunk will try to convert the source data from its encoding into the
  UTF-8 encoding unless I specifiy an alternative character set via the `CHARSET` key in `props.conf`
    - I think this is saying that Splunk has cool algorithms that will correctly change the encoding of non-UTF-8 encoded data into UTF-8, and not
      that Splunk is dumb and will just try to interpet everything as UTF-8?
    - Make sure that an alternative character set exists on the machine
        - View valid character encodings with `$ iconv -l`
- Splunk supports a wide variety of character sets. See the source if this ever becomes a problem
    - Splunk can also be trained to recognized new character sets (see source)
### Example 1
`props.conf`
```
[host::GreekSource]
CHARSET=ISO-8859-7
```
- I specify an alternative charset in `props.conf` for an existing host, called "GreekSource," that generates Greek data
### Example 2
`props.conf`
```
[host::my-foreign-docs]
CHARSET=AUTO
```
- Splunk has built-in algorithms for automatically detecting the proper language and character set encoding which can be used via the `AUTO` value