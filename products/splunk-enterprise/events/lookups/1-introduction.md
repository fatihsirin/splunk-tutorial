- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Aboutlookupsandfieldactions - start of lookup documentation
# Components
## Lookup definition
- In order to use the `lookup` command manually or to use automated lookups, a lookup definition must be created
- A lookup definition is merely a wrapper around a lookup table file that extracts the keys and associated values from the lookup table file and makes
  that mapping available with a set of permissions
  - E.g. the "dc-bus-locations-lookup-defintion" merely extracts the "bus_id", "latitude", and "longitude" column headings from the underlying CSV
    file and makes those headings available as \<lookup fields> with associated values
# Manual lookups
- See the `lookup.md` notes in the distributable-streaming directory for manual lookups