- https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/RemovedatafromSplunk
- https://docs.splunk.com/Documentation/Splunk/7.1.0/Security/Secureyouradminaccount#Create_a_password_when_starting_Splunk_for_the_first_time - edit user-seed.conf
# Hide data via `delete`
- Splunk can "delete" data from an index by piping the results of a search to the `delete` command
- Only users with the `delete_by_keyword` capability can use the `delete` command
  - The admin user does not have this capability by default
- The `delete` command does not:
  - Update the metadata of any events
    - Metadata searches will still include the deleted, unsearchable events
    - The main "All indexed data" dashboard will still show event counts for deleted sources, hosts, or sourcetypes
  - Reclaim disk space
    - The "deleted" events are simply made permanently invisible to any future searches
      - No user, even the admin, will ever be able to see the data again
## Steps
- Confirm that 
- Run a search that generates *only* the events that I want to be deleted
  - Be sure the search is correct!
- Once the results are verified, pipe the results into the `delete` command
# Remove data from disk
- Splunk has a CLI `clean` command to remove data from disk
  - Run `$ splunk help clean` for details
- One or all indexes will be cleaned, depending on the command-line arguments
- Typically, this command is run before re-indexing data
- The command does not work on clusterd indexes
- This command is overkill. Using the `delete` command is faster and is usually sufficient
## Examples
- `$ splunk clean all`
  - Delete all events that were indexed, host tags, source type aliases, user accounts, etc. from all indexes
  - It at leasts gives me a prompt before cleaning
  - This delete all users so that I cannot login to the website anymore!
## Helpful background
- Running $ splunk list index reveals the following indexes in my local Splunk Enterprise installation: _audit, _internal, _introspection,
  _telemetry, _thefishbucket, history, main, splunklogger, and summary
  - Listing the details of each index should be in separate notes
- After deleting everything, there will be no users
  - Fix this by editing `$SPLUNK_HOME/etc/system/local/user-seed.conf`
```
[user_info]
USERNAME = <user name>
HASHED_PASSWORD = <your password>
```
- It is also possible to add a user with the CLI, but this won't work if I deleted all users because I have to be logged-in to add a user! 
  - Syntax: `$ splunk add user <username> -password "<password>" -role <role>`
    - E.g. `$ splunk add user austin -password '1q2w3e4r' -full-name 'Austin Chang' -role admin`
      - The `-full-name` flag is optional
      - `<role>` can be "admin", "power", or "user"