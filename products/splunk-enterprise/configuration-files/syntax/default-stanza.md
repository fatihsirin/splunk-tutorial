- https://answers.splunk.com/answers/656487/will-stanzas-inherit-index-from-default-in-inputsc.html
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Propsconf
# Introduction
- These notes are specifically about the `[default]` stanza in `props.conf`, but they could generalize to other configuration files
# No [default] stanza
- A `[default]` stanza is never required to define global settings. Those settings can be written outside of any stanza at the top of the file
  - Never do this
# Multiple [default] stanzas
- Each configuration file should have at most one `[default]` stanza
- If there are multiple `[default]` stanzas, then their settings will be combined and the last one to be defined in the file takes precedence in the
  case of a conflicting attribute value
# [default] stanza inheritance
- Any attributes defined in the `[default]` stanza of a file will be inherited by all other stanzas
- If an inheriting stanza defines a value for an attribute that already exists in `[default]`, then the inheriting stanza's value for that attribute
  will take precedence
# Important [default] stanza attributes
- `DATETIME_CONFIG = /etc/datetime.xml`