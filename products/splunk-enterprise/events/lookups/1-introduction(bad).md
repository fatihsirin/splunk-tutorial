- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/Aboutlookupsandfieldactions - start of lookup documentation
# Examples
- `sourcetype="oms" | lookup bus-lookup bus_id as bus_affected OUTPUT latitude as lat, longitude as lon`

# Delete
- `sourcetype="oms" | lookup bus-lookup bus_id as bus_affected OUTPUT latitude as lat, longitude as lon | geostats latfield=lat longfield=lon count`