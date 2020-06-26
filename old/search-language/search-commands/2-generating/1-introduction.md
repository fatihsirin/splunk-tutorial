- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - introduction to all command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype#Generating_commands - list of generating commands
# Input and output
- A generating command does not take any input
  - E.g. the `search` command is itself a generating command (only when used first in the SPL)
    - If the `search` command is used later in an SPL statement, then it is a distributable streaming command
  - It fetches information, without any transformations, from indexes and generates either a group of events or a report
- A generating command returns a list or a table
# Performance
- A generating command can be distribuatable or centralized
  - I assume that the type the command falls under will impact performance, but the documentation doesn't explicitly say
- I think that if I need to use a generating command, I won't really have a say in performance anyway (at least as far as can be optimized through
  modifying the search query)
