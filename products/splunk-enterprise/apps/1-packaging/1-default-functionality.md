- https://dev.splunk.com/enterprise/docs/releaseapps/packagingtoolkit - packaging documentation
# Physical partitioning
- By default, Splunk's Packaging Toolkit takes an app's source package and divides it into three deployment packages:
  - `<my-app>.search-head.tgz`: package for search head
  - `<my-app>.indexer.tgz`: package for indexers
  - `<my-app>.forwarder.tgz`: package for forwarders
- Each deployment node is its own "workload" 
- These deployment packages "just work"
  -  I don't have to provide any input to the toolkit to generate these packages, nor to make sure they work properly on their respective node types
# App manifest generation
- An app doesn't *need* a manifest file because Splunk can derive one from other configuration files
- However, an app must have one in order to be configured to be deployed to take advantage of logical partitioning (as opposed to physical
  partitioning)