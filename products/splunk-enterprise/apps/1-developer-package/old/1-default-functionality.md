- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit - packaging documentation
# Physical partitioning
- By default, if an admin chooes to use Splunk's `$ slim parition <app source>` command, the \<app source> will be divided into three deployment
  packages:
  - `<my-app>.search-head.tgz`: package for search head
  - `<my-app>.indexer.tgz`: package for indexers
  - `<my-app>.forwarder.tgz`: package for forwarders
- Each deployment node is its own "workload" 
- These admin-generated deployment sub-packages are Splunk's best guess about where configuration should be stored
  - In the majority of cases, I as a developer don't have to write any additional app configuration to enable the CLI command to correctly generate
    these sub-packages, nor to make sure they work "properly" on their respective node types
  - However, Splunk's best guess isn't always correct! 
  
  Sometimes I have to use input groups ...


    - Just because Splunk gene
    - However, this _does not mean_ the correct configuration will always correc placed into the correct sub-package! Splunk basically does its best guess about where
      things should go, but the admin _must_ understand their topology to partition the app correctly!
# App manifest generation
- `$ slim generate-manifest <app directory> -o <filename>`
  - E.g. `$ slim generate-manifest untopchan2 -o untopchan2/app.manifest`
- An app doesn't *need* a manifest file because Splunk can derive one from other configuration files
- However, an app must have one in order to be configured to be deployed to take advantage of additional logical partitioning (as opposed to just
  physical partitioning)
