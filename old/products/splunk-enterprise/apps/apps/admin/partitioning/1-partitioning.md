- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit/pkgtoolkitref/packagingtoolkitcli/#slim-partition
# Examples
## Create a search-head/indexer sub-package and a forwarder sub-package
### CLI command
```
$ slim partition --combine-search-head-indexer-workloads untopchan-0.0.1.tar.gz 
```
# Purpose
- By default, if an admin chooses to use Splunk's `$ slim parition <app source>` command, the \<app source> will be divided into three deployment
  packages, one for each "workload":
  - `<my-app>.search-head.tgz`: package for search head
  - `<my-app>.indexer.tgz`: package for indexers
  - `<my-app>.forwarder.tgz`: package for forwarders
- This partitioning "just works." The correct configuration files _will_ be divided correctly across the three physical workloads (see
  `2-default-sub-packages.md`)
- I as a developer can write _additional_ app configuration in the `app.manfiest` to enable the admin to perform more granular partitioning across
  logical workloads, _if the admin so chooses_
  - This granular partitioning option is enabled by "input groups" in `app.manifest`, but the developer has to include them for the admin to be able
    to use them!
# Tool management
- The latest working version of the "semantic-version" package is 2.6.0
# Files
- If any file was in local/ when the app was packaged, then it will also be in local/ in this sub-package. As a developer, I almost always want to
  store the "meat" of an app in the app's default/ directory. This packaging command doesn't "fix" this ommission
- What is an installation graph? There is no documentation on this. It appears to be the `installation.json` file, but no such file is generated
  - Instead, `installation-update.json` and `installation-actions.json` are generated. Are these two files the new or old version of
    `installation.json`? What a mess