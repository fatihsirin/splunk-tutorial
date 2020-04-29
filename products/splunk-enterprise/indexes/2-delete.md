- https://docs.splunk.com/Documentation/Splunk/latest/Indexer/RemovedatafromSplunk - various ways to delete an index
# Delete non-clustered index
- `$ ./splunk remove index <index name>`
  - The only other files in $SPLUNK_DB that appeared to be modified were in the fishbucket/ directory, which pretty well confirms that manual deletion
    of an index is a valid approach
- I can also manually edit `indexes.conf` and delete the associated index directories in `/Applications/Splunk/var/lib/splunk/`
# Sending data to a nonexistent index
- If this happens, Splunk doens't report any obvious errors in Splunk Web