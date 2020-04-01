- https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/Aboutindexesandindexers - high-level introduction
# Indexer cluster purpose
- An indexer cluster enables redundant indexing and searching capability
- In order to prevent data loss and increase data availability for searching, I can create a group of indexers that contain identical data.
    - The group of indexers is called an "indexer cluster," and the process of replicating the data is called "index replication"
# Index cluster node types
- There are three types of nodes in an indexer cluster: a master node, peer nodes, and search heads
## Master node
- An indexer cluster always has a single master node that manages the cluster
    - The master node is a specialized type of indexer
## Peer nodes
- An indexer cluster has multiple peer nodes that index the data, maintain multiple copies of the data, and runs searches across the data
## Search heads
- An indexer cluster contains one or more search heads to coordinate searching across the peer nodes
