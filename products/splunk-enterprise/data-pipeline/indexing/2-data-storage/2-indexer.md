- https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/Aboutindexesandindexers - high-level introduction
# Indexer purpose
- An indexer is a Splunk Enterprise instance that indexes the incoming data and searches the indexed data
  - I already knew that an indexer indexes data. What I didn't realize is that an indexer also is responsible for searching its own data against
    queries
- A "search head" is simply a Splunk instance that coordinates searching across a set of indexers
  - Thus, depending on the topology, an indexer could also be a "search head" (unless the term "serach head" is specifically reserved for Splunk
    instances that 1) only search and 2) interact with multiple indexers)
# Topology
- A Splunk instance can have the sole role of being an indexer, or it can handle multiple roles in the data pipeline
  - In a small deployment, a single machine would handle data input, parsing, indexing, and searching
  - In a large deployment, a group of machines would be dedicated to being indexers while other machines would handle input, parsing, and searching