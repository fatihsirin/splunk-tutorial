- https://docs.splunk.com/Documentation/SplunkCloud/8.0.2003/Knowledge/Abouteventtypes - start of event types documentation
# Introduction
- A search can be saved as an event type
- In the future, any event that _would_ be returned by that search will be grouped under that event type at search time
  - In other words, events that were returned by the saved search will always have that event type _even when_ they are found by a completely
    different search
    - This fact right here communicates the usefulness of event types
- Tags can be added to event types
# Purpose
- The purpose of using event type is to gain a clearer understanding of data that I don't already understand
- I can assign event types to search as I go. As I perform searches, hopefully I'll discover relationships between groups of events that I didn't
  realize existed
  - E.g. if `sourcetype=access_combined status=200 action=purchase` is saved as the "successful_purchase" event type, and I search purchases by IP,
    perhaps I could find particular users who make more successful purchases than other users. Then I could investigage why those users had more
    success
    - This is an obvious example, but it illustrates an example of finding new insights in data
## Tags
- Just like \<field>=\<value> combinations can have tags, event types can also have tags
  - E.g. `tag::eventtype=HTTP client error` will return all events of all event types that are tagged with "HTTP client error"
# Files
- An event type is stored as a stanza with `eventtypes.conf`
# Syntax
- `eventtype=<name of saved eventtype>`
  - This will return events that match the specified event type
- An event can have multiple event types
  - If it does, the `eventtype` field will be a multi-value field
- Event types cannot save searches that use a pipe "|" or a subsearch
  - They _shouldn't_ save searches that use the `savedsearch` command to reference a report name
- An event type can be given a numeric priority that will make Splunk process event types 1) by priority and 2) by lexicographical order within event
  types of the same priority
# Performance
- Event types are expensive for Splunk to evaluate because _every_ search must be correlated with _every_ event type!
- Use a search macro if I just want a shortcut for a long search query string
# Workflow
- x