- https://docs.splunk.com/Documentation/Splunk/8.0.2/Knowledge/definecalcfields - calculated fields introduction
# Introduction
- A calculated field is a new field whose data is populated by performing a calculation on one or more existing fields
- A calculated field is just an automated way of running an `eval` expression to generate a new field automatically
# Evaluation order
- All calculated field configurations within the same `props.conf` stanza are evaluated in parallel
  - Therefore, these calculated fields cannot reference each other and there is no "lexicographical ordering" of calcualted fields