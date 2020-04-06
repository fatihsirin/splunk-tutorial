- https://docs.splunk.com/Documentation/Splunk/8.0.2/Search/Typesofcommands - introduction to all command types
- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchReference/Commandsbytype#Dataset_processing_commands - list of dataset processing commands
# Input and output
- A dataset processing command requires the entire dataset of events before the command can generate output
- A dataset processing command can return a variety of output including processed events, a data table, more?
  - E.g. `sort` simply returns the events in sorted order
  - E.g. `fieldsummary` returns a data table of statistics about the search events
# Performance
- Performance must not be great because all the input is required and the command must run on a search head, but the documentation doesn't go into
  detail