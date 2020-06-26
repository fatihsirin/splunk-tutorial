- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - introduction to all command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype#Orchestrating_commands - list of orchestrating commands
# Input and output
- An orchestrating command does not take any input or provide any output
- An orchestrating command controls how a search is processed
  - It does not affect whatever the search returns
# Performance
- An orchestrating command is used to improve the performance of a search
  - It doesn't make sense to evaluate the performance of an orchestrating command itself