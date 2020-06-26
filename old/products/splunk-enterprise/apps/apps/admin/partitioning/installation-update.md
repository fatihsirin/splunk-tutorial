# Examples
## Basic `installation-update.json`
### File
#### `/Users/austinchang/repositories/test-source-code/installation-update.json`
```
{
    "_search_heads":{
        "workload":["searchHead"],
        "apps":{
            "untopchan":{
                "dependencies":[],
                "optional_dependencies":[],
                "dependents":[],
                "is_external":false,
                "is_root":true,
                "inputGroups":{},
                "source":"untopchan-0.0.1.tar.gz",
                "version":"0.0.1"
            }
        }
    },
    "_indexers":{
        "workload":["indexer"],
        "apps":{
            "untopchan":{
                "dependencies":[],
                "optional_dependencies":[],
                "dependents":[],
                "is_external":false,
                "is_root":true,
                "inputGroups":{},
                "source":"untopchan-0.0.1.tar.gz",
                "version":"0.0.1"
            }
        }
    },
    "_forwarders":{
        "workload":["forwarder"],
        "apps":{
            "untopchan":{
                "dependencies":[],
                "optional_dependencies":[],
                "dependents":[],
                "is_external":false,
                "is_root":true,
                "inputGroups":{
                    "*":["untopchan"]
                },
                "source":"untopchan-0.0.1.tar.gz",
                "version":"0.0.1"
            }
        }
    }
}
```
# Purpose
- This file contains a JSON object of JSON objects, each of which represents a server class
- This file is a record of the exact contents of each server class as of the latest partition command
  - This is useful to see the following:
    - The latest version of any apps that were deployed
      - I could, in theory, partition the app inside of a new directory for each deployment to keep a record of the deployments. I could also just
        commit this file to git to see its history
# Files
- This file is overwritten for each new `slim partition` command
  - Since it is overwritten, it is not a historical record of partitioning. It is just a record of what the most recent partition command did