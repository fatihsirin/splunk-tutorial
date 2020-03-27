- https://answers.splunk.com/answers/306886/main-differences-between-splunk-enterprise-and-spl.html
- https://docs.splunk.com/Documentation/SplunkCloud/latest/Service/SplunkCloudservice#Differences_between_Splunk_Cloud_and_Splunk_Enterprise - actual
  detailed differences
- https://answers.splunk.com/answers/691103/what-happens-if-license-volume-limit-is-exceeded.html - what happens if I exceed ingestion limit
- https://www.splunk.com/en_us/products/pricing/calculator.html#tabs/tab2 - pricing calculator

# Introduction
- The main point is that regardless of product, I am charged based on how much data is indexed by Splunk per day
- Splunk no longer offers perpetual licenses, only annual term licenses
- Splunk pricing is intentionally confusing so you have to contact them and give them your contact information
# Splunk Enterprise
- Splunk Enterprise has way more features and is the product I should learn about, since almost everything I learn will also apply to Splunk Cloud
## Pricing
### By data volume
- The price is determined by how much data is indexed in the Splunk installation per day
  - E.g. For an annual license, it costs $1800 per year to ingest data at a rate of 1 GB per day
    - Total price: $1800 * 1 = $1800 at that maximum 1 GB/day rate
- I pay once to index the data. The following are unlimtied:
    - Searches on the data
    - Data storage (with regard to how far back Splunk can use previously ingested data. The company needs to have the infrastructure to hold the Splunk
      indexes)
    - Infrastructure configuration (e.g. unlimited number of nodes, cores, sockets, etc.)
### By infrastructure resources
- Not a lot of detail
# Splunk Cloud
- Splunk Cloud has limited functionality as compared to Splunk Enterprise
## Pricing
### By data volume
- Since Splunk Cloud handles all of the infrastructure, this is the only pricing option
- The price is determined by how much data is indexed by the Splunk platform per day
  - E.g. For an annual license, it costs $1620 per year to ingest data at a rate of 5 GB per day
    - Total price: $1620 * 5 = $8100 per year at that maximum 5 GB/day rate
## Features
### Infrastructure
- The company has no control over the infrastructure that underlies the Splunk Cloud platform
- No CLI access to the infrastructure
### Data sources
- No direct ingestion of TCP, UDP, file, and syslog inputs
    - A Splunk forwarder must be used to forward these types of data to the cloud platform
- Scripted and modular inputs are accepted via the Inputs Data Manager (IDM) or via configured heavy forwarders that the company sets up
### Searching
- A support ticket must be submitted to enable real-time searching
- Real-time searching is disabled by default because it is resource intensive
### REST API access
- A support ticket must be submitted to enable REST API access
- Only a subset of the REST API endpoints are supported in Splunk Cloud