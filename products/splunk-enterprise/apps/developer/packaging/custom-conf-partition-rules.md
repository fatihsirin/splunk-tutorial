- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit#Creating-placement-rules-for-custom-conf-files
# Example
## File
### X
```
<example>
```
# Purpose
- Custom placement directives (e.g. `@placement forwarder`) can be placed within _custom_ `*.conf` files to direct `$ slim partition <app.tar.gz>` where to
  place the stanzas underneath the placement directive
- Placement directives _cannot_ be used with the standard `*.conf` files
  - I tried using them in `indexes.conf` and `props.conf` but I could not