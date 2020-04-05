- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Specifyinputpathswithwildcards - ellipsis pattern
# Introduction
- Input specifications within `inputs.conf` do *not* use regular expressions
  - Instead, they can use Splunk-defined wildcards
- Other files, like `props.conf`, can use wildcards in addition to regular regular expressions
# Wildcards
## Ellipsis
- The `...` wildcard recurses through one or more directories to find a match
  - E.g. `.../untopchan/*` matches any file within any directory called "untopchan" that is at least one directory below `$SPLUNK_HOME`
    - E.g. It matches ``