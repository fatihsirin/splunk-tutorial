- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit - packaging documentation











- Do I want to refactor these notes into the example-centered style? I need to successfully write an input group first. That's what I should have done
  before even writing these notes. That's why these notes took so long
# Logical partitioning
- Splunk's CLI `$ slim partition <app source>` command automatically splits an already-packaged app's source code into three deployment packages: one
  for search heads, one for indexers, and one for forwarders
  - The app package is split along "physical boundaries" because the code is split and distributed according to the inherent role of a node in a
    Splunk topology
- I think that "physical" partitioning should be sufficient for most apps. However, Splunk offers an additional type of partitioning between logical
  groups of data inputs
## Developer and Admin
- Two human roles are required to take advantage of Splunk's "logical" partitioning: the developer and the admin
- If certain data inputs inputs in the app use the same custom parsing code/configuration, developers can elect to group data inputs togthether by
  writing additional configuration code
  - These groups of data inputs (and the associated configuraiton code that groups them together) are called "input groups"
- The developer can write configuration code in their app so that each input group can get its own special package _if_ the admin installs the app
  that way
- Admins can elect to install these input group configurations on selected server classes that they want to be responsible for the associated data
  inputs
  - If the admin does this, only the indexers/forwarders that need the code/configuration will have it
- Admins can also elect to ignore logical input groups defined by the developer entirely and simply give every indexer/forwarder every piece of
  configuration
  - They would do this by ignoring the `$ slim partition <app source>` command and just installing the app on every Splunk node
  - Doing this will make it harder to fix issues and tune performance later on in a complex topology. Logical partitioning exists to group related
    code together so that's it's easier to debug later
# Input groups
- Input groups are created by objects in an explicitly provided `app.manifest` JSON file
- Input groups are typically defined so that they *can* be installed on particular groups of forwarders (a.k.a. server classes) by an admin
  - E.g. a developer can create a dedicated input group for forwarders that deal with Microsoft Exchange data. This input group would then be packaged
    as its own special package, in addition to the three "physical" packages. An admin can decide to deploy the input group to a server class, or
    ignore it entirely
- Recall that `inputs.conf` contains data sources that are named with stanzas. *These* are the existing inputs that can be organized into input groups
- The usefulness of input groups can be understood through the attributes that can be defined for an input group
## Input group schema
- These are a few of the schema attributes
### "inputs" (required)
- "inputs" lists the specific inputs in `inputs.conf` that fall within this input group
  - The schema says this attribute is optional, but what's the point of defining an input group with no underlying inputs?! 
    - There is none. This attribute is _effectively_ required. Why? Because an input group wouldn't apply to anything without referring to some inputs
      in `inputs.conf`
```
<example>
```
### "requires" (optional)
- "requires" lists other input groups that need to be deployed on the same server classes as _this_ input group
  - In other words, it lists the dependencies of this input group (those dependencies are external inputs groups)
  - I'm not sure why an app group and app name are required. Perhaps they are needed for Splunk to know where to look for these external input groups?
    - A Splunk app itself can have dependencies that are other apps. These external apps are specified by the "dependencies" attribute. _These_ apps
      can also have their _own_ input groups. It is _those_ inputs groups that are listed here and identified by the \<app group> and \<app name>
      attributes
- Thus, the "requires" key does _not_ perform dependency resolution. It merely works with the "dependencies" key (which _does_ perform dependency
  resolution) to organize inpunt groups
```
<example>
```