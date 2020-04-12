- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - introduction to all command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype#Transforming_commands - list of transforming commands
# Input and output
- A transforming command requires all of the events from all of the indexers as input
- A transforming command outputs a data table that can be used for statistical and visualization purposes
  - More specifically, it outputs data structures containing numerical values that are required for those purposes
# Performance
- Transforming commands are non-streaming. This means that all of the processing must be performed in a search head
  - This requirement causes a lot of data movement and a loss of parallelism that contribute to slower performance
- Additionally, all of the input must be supplied before any output will be generated