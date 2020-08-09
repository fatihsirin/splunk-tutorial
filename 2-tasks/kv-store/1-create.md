- https://dev.splunk.com/enterprise/docs/developapps/manageknowledge/kvstore/usingconfigurationfiles
# Examples
## Create a KV Store Collection
### Files
#### ?
```
[kvstorecoll]
enforceTypes = true
field.name = string
field.id = number
field.address_street = string
field.address_city = string
field.address_state = string
field.address_zip = string
```
- Creating a KV Store collection via `collections.conf` _is_ how to use the KV Store
- Each top-level stanza creates a new collection (i.e. database table) in the context of the app
  - The stanza name _is_ the collection name