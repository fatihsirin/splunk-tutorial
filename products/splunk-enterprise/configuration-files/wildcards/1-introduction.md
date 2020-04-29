https://answers.splunk.com/answers/397051/can-you-regex-or-wildcard-props-sourcetype-stanzas.html
# I cannot use regular expressions, nor wildcards, inside of sourcetype stanzas
- Sourcetype stanzas in `props.conf` DO NOT SUPPORT REGEX NOR WILDCARD MATCHING. I spent 30 minutes trying to figure out why it wasn't working before
  finding the source above
# I can use regular expressions and wildcards inside of source or host stanzas
- They work fine
# Remember to continuously monitor a data input to write anything to `inputs.conf`
- Indexing a file one time does not write anything to `inputs.conf`