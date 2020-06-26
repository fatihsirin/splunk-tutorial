- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationfilestructureandsyntax
- https://en.wikipedia.org/wiki/INI_file
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Specifyinputpathswithwildcards - ellipsis pattern in Splunk
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Propsconf - props.conf
# Introduction
- Splunk configuration files appear to be based on INI files
- The INI file "format" is very loose
    - E.g. some software treats attribute keys as case-sensitive while others do not
- Splunk configuration files can have stanzas with precedence
    - This is *not* standard in the INI format
    - Some stanzas are case-sensitive while others are not; read the example files
## Example
- `outputs.conf`
```
[tcpout]
indexAndForward=true
compressed=true

[tcpout:my_indexersA]
compressed=false
server=mysplunk_indexer1:9997, mysplunk_indexer2:9997

[tcpout:my_indexersB]
server=mysplunk_indexer3:9997, mysplunk_indexer4:9997
# this is a comment
```
- In this example, which is used to configure Splunk forwarders:
    - The global `[tcpout]` affects all tcp forwarding
    - The two `[tcpout:<target_list>]` stanzas, whose settings affect only the indexers defined in each target group
# Stanzas
- Each configuration file has different sections, called stanzas
- A stanza is denoted by a header in square brackets
    - E.g. `[SSL]`
- Stanzas can be subdivided
    - Settings within more specific stanzas take precedence
        - E.g. `compressed=false` takes precedence in the above example
## props.conf syntax
- This isn't stated anywhere, but I get the feeling that `props.conf` contains example syntax for *all* the rest of the configuration files
- See the source and search for "..."
### stanza regex-like syntax
- A single period `.` is not a wildcard, and is the regular expression equivalent of `\.`
- `...` matches any number of characters, or recurses through directories until a match is found 
    - E.g. `[source::....(?<!tar.)(gz|bz2)]`
        - This matches any file ending with '.gz' or '.bz2', provided this is not preceded by 'tar.', so tar.bz2 and tar.gz would not be matched.
        - I think the first 3 periods are treated as an ellipsis while the fourth one is a literal "."?
- `*` matches anything in a specific directory segment
    - It's not recursive
# Key-value pairs
- There are key-value pairs within a stanza
- Keys are case sensitive 
    - E.g. `sourcetype = my_app` is not the same as `SOURCETYPE = my_app`
- Spaces between the equals signs don't matter
# Comments
- Never put comments on the same line as a key-value pair or a stanza
    - E.g. `a_setting = 5  #5 is the best number`
        - This sets `a_setting` to the value `5 #5 is the best number`, and not to `5` as intended