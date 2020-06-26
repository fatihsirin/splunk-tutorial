# Examples
## Direct forwarder-indexer communication
### File
#### /opt/splunk/etc/system/local/inputs.conf
```
[default]
host = splkbdpidx1.localdomain

[splunktcp-ssl:9997]
disabled = 0

#[tcp-ssl:9997]

[SSL]
...
```
- When a Splunk indexer is configured as a receiver to communicate directly with a Splunk universal forwarder, the following must be true:
  - No stanza can be commented out. This breaks everything for some reason. It must not count as a comment?

  - The `[]` stanza ...
  - The `disabled = 0` attribute _must_ be set for ...