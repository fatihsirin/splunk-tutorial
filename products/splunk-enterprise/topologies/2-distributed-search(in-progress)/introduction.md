- https://docs.splunk.com/Documentation/Splunk/latest/DistSearch/Whatisdistributedsearch - start of distributed search documentation
- https://docs.splunk.com/Documentation/Splunk/latest/Deploy/Searchheadwithindexers - a perfect example of the AIFBDP topology
- https://answers.splunk.com/answers/33996/adding-an-index-in-distributed-setup.html - how to add indexes in a distributed search
- https://docs.splunk.com/Documentation/Splunk/8.0.3/DistSearch/Forwardsearchheaddata - very brief mention of a possible relationship between add-ons
  and indexer settings
- https://docs.splunk.com/Documentation/AddOns/released/Overview/Add-onsandindexes - relationship between add-ons and indexes

- The distributed search topology comprises one or more search head nodes that only perform search management and presentation functions. The indexers
  (a.k.a. "search peers") index their data _and_ search across their data
  - A distributed search topology can include search head clusters and indexer clusters, or neither
  - If I want to use more than one search head, I should use a search head cluster

- I already know that a search head gets data from the search peers. However, the search head _also_ replicates its own knowledge objects and sends
  them to each search peer. This set of knowledge objects is called the "knowledge bundle"
- Unless the knowledge bundle is modified, it comprises almost the entirety of the search head's app/ directory
  - That means each indexer will get nearly a full copy of the app/ directory

- Since knowledge bundle replication is so extensive, why bother partitioning an app into separate sub-packages and deploying the sub-packages with
  the deployment server?
  - A search peer stores the received knowledge bundle in `$SPLUNK_HOME/var/run/searchpeers`. This is obviously different from the
    `$SPLUNK_HOME/etc/deployment-app/` directory, so I guess there's some organizational value?

- Search peers must explicitly be added to the search head
  - This cannot be done with the deployment server. That would be too easy, wouldn't it!?
- Fortunately, this can be done in Splunk Web

- It is best practice to disable indexing entirely on the search head and configure the search head to forward any data it gets to the search peers
  - In other words, the search head should be configured to _be_ a forwarder
  - Why would I ever configure a script (or some other data input) directly on the search head that would necessitate this? That seems really dumb.
    But if I did, I should do this

- The perfect example source does not describe how to create indexes on the different indexes
  - I have to SSH into each indexer and create the indexes
  - Alternatively, ...

# This makes my head spin
- The workflow for _maintaining_ an app is continuous and is as follows:
  - Develop the app in its entirety locally on a single Splunk Enterprise instance and package it
  - Partition the app into three sub-packgages: one for search heads, indexers, and forwarders
  - Use the deployment server to deploy each sub-package as an app to the relevant server class
    - I'm assuming there's one server class for indexers and one for forwarders
- The workflow for _installing_ the data for an app should only need to occur once. It is as follows:
  - Log into each indexer and create the desired indexes
  - Ensure the app is easy to configure to work with these indexes
    - E.g. include search macro in each Dashboard panel that an admin could use
- This workflow is dumb but necessary. In a perfect world, an app would simply _consume_ and present an agreed-upon-in-advance, common data format.
  _How_ this data was indexed and _where_ it was sourced from would be _entirely_ up to the admin. I suppose I'm just confused because I'm both the
  admin and the developer in this scenario


XXX
- This is all very well and good, but how do I manage _indexes_ on search peers?