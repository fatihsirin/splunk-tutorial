- An app can have invalid configuration spread across `app.conf`, `app.manifest`, and other files
- An app should _fail_ to package entirely if any configuration is wrong
- The developer who made the changes should immediately be notified if packaging fails
- Therefore, `slim package` must be run in the local repository before changes are pushed

- There are no guarantees about how an admin wants to deploy an app
- Therefore, it does not make sense to store post-partition sub-packages in the git repository
  - What if the admin wants to perform a different partition scheme? Having those pre-existing sub-packages would be confusing
- It is also bad to store 

# aifbdp-splunk-apps git branches
## master branch
```
aifbdp-splunk-apps (repo)
  src/
    untopchan/
    x/
```
## distributed-search branch
  package.sh?
```
aifbdp-splunk-apps (repo)
  distributed-search-partition.sh?
  deployment-apps/
    untopchan/
      untopchan-forwarders/
      untopchan-indexers/
    x/
      x-forwarder/
      x-indexer/
  apps/
    untopchan/
      untopchan-search-heads/
    x/
      x-search-head/



```

# Workflow
- One person does all of the following steps:
  - Locally makes changes (and verifies they are correct by running a test `slim package` command on the app src folder)
  - Commits app src changes to the aifbdp-splunk-apps master branch
  - If those changes are ready for production, then do the following:
    - Run `slim package` to generate the full app package
    - Don't add/commit the app.tar.gz file to the master branch
    - Switch to the branch for the desired deployment scheme (e.g. distributed search)
    - Partition the full app package as desired
    - For each sub-package:
      - Extract the outer tarball
      - Extract the inner tarball
      - According to the sub-package suffix (e.g. "forwarders"), rename the inner folder and then move it
    - Delete the sub-package *.tar.gz files, the directories extracted from the outer tarballs (they have annoying version numbers)
    - Commit changes to `installation-actions.json` and `installation-update.json`