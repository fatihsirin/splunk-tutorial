- https://docs.splunk.com/Documentation/Splunk/8.0.3/Indexer/Aboutclusters
# Purpose
- An indexer cluster is a group of Splunk Enterprise _indexers_ (not indexes) that are configured to all contain the exact same data
- Each node in an indexer cluster is like a regular stand-alone Splunk Enterprise indexer machine instance. The difference is that there is a master
  node which coordinates different indexer machine instances so that they copy each other's data while also indexing data normally