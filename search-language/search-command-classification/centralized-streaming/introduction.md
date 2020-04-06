- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - introduction to all command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype#Streaming_commands - list of streaming commands
# Input and output
- A centralized streaming command accepts as input each event, one at a time, that was returned by a search
- The command outputs zero or one event
# Performance
- A centralized streaming command must be run on a search head because the order of the input events matters
- Centralized streaming commands have a performance advantage over non-streaming commands in that they can yield output before the final input is
  entered
  - The documentation does not detail why this is