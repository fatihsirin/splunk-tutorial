- https://docs.splunk.com/Documentation/Splunk/latest/Troubleshooting/WhatSplunklogsaboutitself
# Examples
## Search all internal logs
### Searches
- `index=_internal`
## Search splunkd.log logs within the _internal index
### Searches
- `index=_internal sourcetype=splunkd`
## Search introspection logs
### Searches
- `index=_introspection`
## Search by log level
### Searches
- `index=_internal (log_level=error OR log_level=warn*)`
# Purpose
- Generally, the $SPLUNK_HOME/var/log/splunk/splunkd.log is what I'm most interested in searching
  - stderr messages created by scripts and similar are logged here
## Log types
- $SPLUNK_HOME/var/log/splunk/ contains Splunk software internal logs
  - All of this information is stored in the "_internal" index
- $SPLUNK_HOME/var/log/introspection/ contains information about the impact of Splunk on the host system
  - All of this information is stored in the "_introspection" index
- $SPLUNK_HOME/var/run/splunk/dispatch/ contains search logs
  - These logs are not indexed by default
- See source for more details
# Syntax
## Log levels
- Splunk has five log levels: DEBUG, INFO, WARN, ERROR, FATAL