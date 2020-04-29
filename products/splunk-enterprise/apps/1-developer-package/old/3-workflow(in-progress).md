- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit/#Packaging-Toolkit-workflows - packaging documentation
- https://docs.splunk.com/Documentation/Splunk/8.0.3/Admin/Managingappobjects - how to install an app for admins
# Developer
- `$ slim generate‑manifest <app root dir> [-o <output file>]`
  - E.g. `$ slim generate-manifest untopchan -o ./untopchan/app.manifest`
  - Generate the contents of the `app.manifest` file, optionally outputing to a specific file
- Add content to the stub sections of the file, if desired
- `$ slim validate (<app route dir> | <app .tgz package>)`
  - E.g. `$ slim validate untopchan`
  - Validate the app manifest
- `$ slim package <app route dir>`
  - E.g. `$ slim package untopchan [-o <output directory>`
  - Package the app, optionally outputing to a directory other than cwd
- Now the .tgz file can be handed to off a Splunk admin. Easy!
# Admin
## Partition the app package
- `$ slim partition <app .tgz file>`
  - E.g. `$ slim partition untopchan-1.0.0.tar.gz`
  - By default, this command creates three sub-packages: one for search heads, one for indexers, and one for forwarders
- The latest working version of the semantic-version package is 2.6.0

## 


  ```
    - I need to understand the search evaluation order to know exactly how to package this app. For example, when are lookups resolved? When are
      sourcetypes used? Once I understand when/where the components of the app are used, I can generate the appropriate packages
  ```
  - Partition the app source package into separate packages for each node type _after_ checking for dependency conflicts are resolving/reporting them
    - _This_ command is how a Splunk admin splits the app source into packages according to their specific topology
  - There are a _lot_ of options with this command
    - E.g. The search head and indexer packages can be merged into a single package!
- 
```
Keep listing steps here
```
# Additional considerations
- Custom `*.conf` files require placement rules inside of them for the CLI to know where to package them
- It is possible to use dependencies that have different versions for each OS
- By default, all stanzas in `inputs.conf` are partitioned into the forwarder deployment package
  - To instead partition a stanza into the search head package, uses the "tasks" attribute in `app.manifest`
- Use the "targetWorkloads" attribute to specify that an app should be partitioned into less than the 3 default packages