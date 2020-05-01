- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Deployappsandadd-ons - doesn't really say anything about app deployment
- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit/pkgtoolkitref/packagingtoolkitcli#slim-partition - partition command
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationparametersandthedatapipeline - mapping of `props.conf` settings to Splunk pipeline phases
# `$ slim partition`
```
$ slim partition <app source package>
```

## Examples
### Generate three deployment sub-packages
- `$ slim partition untopchan-1.0.0.tar.gz`
### Generate two deployment sub-packages
- `$ slim partition -c untopchan-1.0.0.tar.gz`
# Purpose
