- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - introduction to all command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype#Streaming_commands - list of streaming commands
# Input and output
- A distributable streaming command accepts as input each event, one at a time, that was returned by a search
- A distributable streaming command outputs zero or one event
# Performance
- A distributable streaming command is the only type of command that can be run on an indexer instead of a search head
- The command can be run on an indexer because the order of the input events does not matter
  - Each indexer can apply the command without consulting any other indexer or search head
    - Thus, the command can be applied to subsets of indexed data in a parallel manner
- The ability to delegate processing to an indexer is why distributable streaming commands *can* have great performance
  - Whether or not a distributable streaming command actually runs on an indexer depends on the other commands in the search
    - If all of the commands before the distributable streaming command can be run on an indexer, then the distributable streaming command is run on
      the indexer
    - If any one of the commands before the distributable streaming command must be run on a search head, the reamaining commands must all be run on
      the search head
      - This is because once a search moves to a search head, it cannot move back to an indexer
