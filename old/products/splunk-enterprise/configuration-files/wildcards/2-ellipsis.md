- https://docs.splunk.com/Documentation/Splunk/8.0.3/Data/Specifyinputpathswithwildcards - wildcard documentation
# Examples
## Match example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::/Users/austinchang/repositories/untopchan/foo_bar.json]
FIELDALIAS-foo = hello ASNEW foo_hello

[source::/Users/...]
FIELDALIAS-bar = hello ASNEW bar_hello

[source::.../Users/...]
FIELDALIAS-baz = hello ASNEW baz_hello

[source::...Users/...]
FIELDALIAS-boo = hello ASNEW boo_hello
```
### Searches
- `sourcetype="circuitComponent"`
## Path non-match example
### File
#### `/Applications/Splunk/etc/apps/untopchan/local/props.conf`
```
[source::/Users/austinchang/repositories/untopchan/.../foo_bar.json]
FIELDALIAS-foo = hello ASNEW foo_hello

[source::/Users/austinchang/repositories/untopchan/...foo_bar.json]
FIELDALIAS-bar = hello ASNEW bar_hello
```
### Searches
- `sourcetype="circuitComponent"`
# Purpose
- The ellipsis `...` wildcard character matches any character, including path separators, zero or more times
  - In the first match example, all four stanzas match, even those that put the ellipsis at the front of the source name
- If `...` has nothing to match, it doesn't match anything!
  - This seems obvious, but consider the path non-match example. In the first stanza, `...` doesn't match anything, so the evaluated source name
    becomes "/Users/austinchang/repositories/untopchan//foo_bar.json". The actual source name doesn't have two consecutive path separators, so the net
    result is that the first stanza doesn't match at all!
    - The second stanza matches because even though `...` doesn't match anything, there is only a single path separator
# Files
- Source and host stanzas (where `...` can be used) are defined within `props.conf`
- `...` can also be used in input stanzas with `inputs.conf`