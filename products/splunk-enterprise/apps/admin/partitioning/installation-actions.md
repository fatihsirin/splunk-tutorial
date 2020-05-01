# Examples
## Basic `installation-actions.json`
### File
#### `/Users/austinchang/repositories/test-source-code/installation-actions.json`
```
[
    {
        "serverClass":"_search_heads",
        "remove":null,
        "add":"/Users/austinchang/repositories/test-source-code/untopchan-0.0.1-_search_heads.tar.gz"
    },
    {
        "serverClass":"_indexers",
        "remove":null,
        "add":"/Users/austinchang/repositories/test-source-code/untopchan-0.0.1-_indexers.tar.gz"
    },
    {
        "serverClass":"_forwarders",
        "remove":null,
        "add":"/Users/austinchang/repositories/test-source-code/untopchan-0.0.1-_forwarders.tar.gz"
    }
]
```
# Purpose
- This file contains JSON objects, each of which represents a server class
- This file is a record of what was added and removed from each server class during the last partition command
  - This is useful to see the following:
    - What, if any, apps were removed from a server class
      - I could, in theory, partition the app inside of a new directory for each deployment to keep a record of the deployments. I could also just
        commit this file to git to track its history
# Files
- This file is overwritten for each new `slim partition` command
  - Since it is overwritten, it is not a historical record of partitioning. It is just a record of what the most recent partition command did