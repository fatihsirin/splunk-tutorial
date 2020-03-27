- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - explanation of command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype - command type classifications
# Command types
- There are six broad categories of commands (I added what I believe to be their relative usage frequency):
    - distributable streaming
        - Common
    - centralized streaming
        - Uncommon
    - transforming
        - Common
    - generating
        - Rare
    - orchestrating
        - Rare
    - dataset processing
        - Uncommon
- A command may fall under one or more of these categories
- Streaming commands operate on single events: they get one event as input and output zero or one event
- Non-streaming commands operate on all of the events returned by the search
    - Many transforming commands are also non-streaming commands
    - These commands force all data to the search head, so they cause a loss of parallelism and lots of data movement
## Distributable streaming
- The order of the events does not matter
### Performance
- This is the most efficient type of streaming command because it can process each event directly in its indexer
    - However, the rest of the commands in the search determine whether or not this will occur
        - If all of the commands before the distributable streaming command can be run on an indexer, then the distributable streaming command will
          also run on the indexer
        - If any one of the commands before the distributable streaming command must be run on the search head, then all reamining commands must be
          run on the search head
            - Once a search has moved into the search head, it cannot move back to an indexer
- E.g. `eval`
## Centralized streaming
- The order of the events matters
- A centralized streaming command can only be run on the search head
## Transforming
- All of the search results are ordered within a data table
- E.g. `top`
## Generating
- A generating command fetches information from the indexers, without any transformations
- Generating events either create events themselves or generate reports
    - Most report-generating commands are also centralized. These commands return a list or table
- These commands don't take any input
    - They are typically placed at the beginning of a search with a leading pipe
- The `search` command is implicitly used in the search bar and is a generating command
## Orchestrating
- An orchestrating command controls how a search is performed, but it does not affect the results returned by the search
## Dataset processing
- A dataset processing command requires the entire dataset before the command is performed
- E.g. `sort`