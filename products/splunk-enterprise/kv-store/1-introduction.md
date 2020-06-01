- https://docs.splunk.com/Documentation/Splunk/latest/Admin/AboutKVstore - start of KV Store documentation
- https://answers.splunk.com/answers/305372/which-version-of-mongodb-does-splunk-enterprise-us.html - Splunk Enterprise uses MongoDB
- https://dev.splunk.com/enterprise/docs/developapps/kvstore/usingconfigurationfiles/ - how to create a KV Store collection
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Serverconf#KV_Store_configuration - how to configure the KV Store in `server.conf`
# Purpose
- The purpose of the app key value Store (a.k.a. KV Store) provides a way to save the state of a Splunk application
  - It acts like a database. In fact, it _is_ implemented using MongoDB!
# Usage workflow
- An app can _choose_ to use the KV Store by doing the following:
  - Use the REST API or configuration files to create a KV Store collection
  - Perform CRUD operations on the KV Store using search commands or the REST API
  - Manage collections using the REST API
# Files
- According to the documentation for `server.conf`, which configures the KV Store, the KV Store is stored at `$SPLUNK_DB/kvstore`, which on my macOS
  machine translates to `$SPLUNK_HOME/var/lib/splunk/kvstore`
  - Unfortunately, the KV Store files are _not_ text files
  - Splunk uses MongoDB to implement the KV Store. I would like to learn about MongoDB!


- These notes are perfect. This is as much information as an introduction should _ever_ contain. Introductions should be very high level to the point
  where creating an example would be not useful or harmful