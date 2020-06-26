- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationparametersandthedatapipeline - data pipeline high-level introduction and
  configuration files
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Deploy/Datapipeline - actual detailed descriptions of the phases
# Data pipeline phases
- The data pipeline, within which raw data is indexed to become searchable, has four phases:
    - Input
    - Parsing (i.e. "event processing" which is grouped under the indexing phase)
    - Indexing
    - Search
- The proper configuration files must be modified on the machine handling the associated data pipeline phase
  - E.g. modify inputs.conf on the machine handling the input phase
  - E.g. modify props.conf on the machine handling the parsing phase (and the machine handling the input phase)
  - E.g. modify indexes.conf on the machine handling the indexing phase
# Mapping Splunk Enterprise components to phases
- Data only moves through each phase of the pipeline once, full stop
- If a single machine weere serving as the indexer and search head, then it would perform all four phases
  perform the phase)
- If there there were Splunk topology with different machines serving different roles, then the pipeline would extend across all of those machines
  - E.g. a set of universal forwarders would handle the input phase, then forward the data to a heavy indexer which would do the parsing phase.
    Finally, the heavy forwarder would send data to the indexer to handle the indexing phase
- This matters because the correct configuration parameters for a phase need to set on the component(s) that are executing the phase
  - E.g. input configuration files could need to be modified on the universal forwarder(s) *or* the indexer(s) depending on how the topology is set up
## Input phase
- The input phase can be executed by an indexer, universal forwarder, or heavy forwarder
## Parsing phase
- The parsing phase can be executed by an indexer, heavy forwarder, or a univesal forwarder (special case)
## Indexing phase
- The indexing phase can only be executed by an indexer
## Search phase
- The search phase can be executed by an indexer or a search head