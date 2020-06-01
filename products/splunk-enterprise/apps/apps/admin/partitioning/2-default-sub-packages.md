# Sub-package contents
## \<app name>-\<version>-_search_heads.tar.gz
### File
#### untopchan-0.0.1-_search_heads.tar.gz
```
untopchan-0.0.1-_search_heads/
  untopchan/
    bin/
      README
      run_untopchan.sh
    default/
      app.conf
      data/
        ui/
          nav/
            default.xml
          views/
            README
              scada.xml
              oms.xml
      indexes.conf
      props.conf
      tags.conf
      transforms.conf 
    lookups/
      bus-locations.csv
      line-ratings.csv
    metadata/
      default.meta
```
- This sub-package contains the following notable files:
  - Complete `app.conf`
  - Complete dashboard XML files 
  - Complete `indexes.conf` 
  - Complete `transforms.conf`
  - Partial `props.conf`
    - Includes search-time source type field extraction attributes
      - E.g. `KV_MODE`, `category`, `description`, `disabled`, inline extractions, and transform extractions
    - Includes field aliases
    - Excludes index-time source-based source type assigment attributes
  - Complete `tags.conf`
  - A lookups directory with the complete CSV lookup files
- This sub-package does not handle:
  - The input phase. Therefore it lacks `inputs.conf`
  - The indexing phase. Therefore source stanzas in `props.conf` are empty
## \<app name>-\<version>-_indexers.tar.gz
### File
#### untopchan-0.0.1-_indexers.tar.gz
```
untopchan-0.0.1-_indexers/
  untopchan/
    bin/
      README
      run_untopchan.sh
    default/
      app.conf
      indexes.conf
      props.conf
      transforms.conf 
    metadata/
      default.meta
```
- This sub-package contains the following notable files:
  - Partial `app.conf`
    - Lacks `[ui]` stanza
  - Complete `indexes.conf`
  - Partial `transforms.conf`
    - Includes index-time source-based source type assignment regex stanzas
    - Excludes lookup stanza contents, and search-time transforms field extractions
  - Partial `props.conf`
    - Includes index-time source-based source type assigment attributes
      - E.g. `DATETIME_CONFIG`, `LINE_BREAKER`, `SHOULD_LINEMERGE`, `TRANSFORMS-` and `disabled`
    - Excludes search-time source type field extraction attributes
- This sub-package does not handle:
  - The input phase. Therefore it lacks `inputs.conf`
  - Search-time derived fields. Therefore it lacks lookups and `tags.conf`
  - Search display. Therefore it lacks any display xml files (e.g. dashboards)
## \<app name>-\<version>-_forwarders.tar.gz
### File
#### untopchan-0.0.1-_forwarders.tar.gz
```
untopchan-0.0.1-_forwarders/
  untopchan/
    bin/
      README
      run_untopchan.sh
    default/
      app.conf
      inputs.conf
      props.conf
    metadata/
      default.meta
```
- This sub-package contains the following notable files:
  - Partial `app.conf`
    - Lacks `[ui]` stanza
  - Complete `inputs.conf`
  - Partial `props.conf`
    - Includes `pulldown_type` attribute which affects source type visibility, as well as __input phase__ _and_ __structured parsing phase__
      configuration attributes
      - E.g. source type stanzas with their `NO_BINARY_CHECK` and `INDEXED_EXTRACTIONS` attributes
    - Excludes everything else!
- This sub-package does not handle:
  - Anything other than the input phase
# Purpose
- Each sub-package contains the subset of an app's files that should be sent to the respective server class
- By default, the `$ slim partition` command creates three sub-packages: one for search heads, one for indexers, and one for forwarders
  - Fun fact: each \<app name and version>.tar.gz file contains a folder of the exact same name. This is expected. What's more interesting is that
    _this_ folder contains _another_ folder that is just the regular name of the app. In this way, every Splunk node will have the an app folder in
    the same relative location, but the contents of that folder will be different on every node!
    - E.g. "untopchan-1.0.0-search_heads.tar.gz" contains a folder "untopchan-1.0.0-search_heads" which contains another folder "untopchan" which
      contains stripped-down .conf files
- These sub-packages are installed on their respective Splunk node types by a Splunk admin