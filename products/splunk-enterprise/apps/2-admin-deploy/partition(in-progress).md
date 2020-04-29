- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Deployappsandadd-ons - doesn't really say anything about app deployment
- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit/pkgtoolkitref/packagingtoolkitcli#slim-partition - partition command
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationparametersandthedatapipeline - mapping of `props.conf` settings to Splunk pipeline phases
# `$ slim partition`
```
$ slim partition <app source package>
```
- The latest working version of the "semantic-version" package is 2.6.0
## Examples
### Generate three deployment sub-packages
- `$ slim partition untopchan-1.0.0.tar.gz`
### Generate two deployment sub-packages
- `$ slim partition -c untopchan-1.0.0.tar.gz`
# Purpose
- By default, the `$ slim partition` command creates three sub-packages: one for search heads, one for indexers, and one for forwarders
  - Fun fact: each \<app name and version>.tar.gz file contains a folder of the exact same name. This is expected. What's more interesting is that
    _this_ folder contains _another_ folder that is just the regular name of the app. In this way, every Splunk node will have the an app folder in
    the same relative location, but the contents of that folder will be different on every node!
    - E.g. "untopchan-1.0.0-search_heads.tar.gz" contains a folder "untopchan-1.0.0-search_heads" which contains another folder "untopchan" which
      contains stripped-down .conf files
- These sub-packages are then installed on their respective Splunk node types
# Output
## Generated files
- If any file was in local/ when the app was packaged, then it will also be in local/ in this sub-packge. As a developer, I almost always want to
  store the "meat" of an app in the app's default/ directory. This packaging command doesn't "fix" this ommission
### \<app name and version>-_search_heads.tar.gz
- This sub-package contains the following notable files:
  - Complete dashboard XML files 
  - A `props.conf` which contains __search phase__ configuration attributes
    - It contains the source type stanzas with their `KV_MODE`, `category`, `description`, `disabled`, and inline extraction attributes
    - It contains source stanzas with field aliases
  - A `transforms.conf` which contains full lookup information
  - A lookups directory with the complete CSV lookup files
  - Complete `tags.conf`
- This sub-package does not handle the input phase, so it lacks that configuration
  - It does not contain an `inputs.conf`
### \<app name and version>-_indexers.tar.gz
- This sub-package contains the following notable files:
  - Complete dashboard XML files
    - These must be here because a Splunk view could be used on any node type?
  - A `props.conf` which contains __parsing phase__ configuration attributes
    - It contains source type stanzas with their `DATETIME_CONFIG`, `LINE_BREAKER`, `SHOULD_LINEMERGE`, and `disabled` attributes
  - A `transforms.conf` which contains empty stanzas 
- This sub-package does not handle the input phase, so it lacks that configuration
  - It does not contains an `inputs.conf` 
### \<app name and version>-_forwarders.tar.gz
- This sub-package contains the following notable files:
  - Complete dashboard XML files
  - A `props.conf` which contains __input phase__ _and_ __structured parsing phase__ configuration attributes
    - It contains source type stanzas with their `NO_BINARY_CHECK` attribute
    - It contains source type stanzas with their `INDEXED_EXTRACTIONS` attribute
- This sub-package does not handle the parsing phase nor the search phase, so it lacks that configuration
  - It does not contain a `transforms.conf`
### installation-actions.json
- This file is generated when an app package is partitioned, but I don't know what it does
### installation-update.json
- This file is generated when an app package is partitioned, but I don't know what it does