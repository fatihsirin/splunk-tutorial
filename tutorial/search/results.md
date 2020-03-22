# Result tabs
- There are four tabs within which search results can be displayed
    - Events
    - Patterns
    - Statistics
    - Visualization
- The type of search commands that I use determine which tab the search results appear in
## Events
- The events tab shows a timeline of events, events themselves, and ways to view the events
- The interface is quite intuitive
### Field types
- Selected fields are visible in my search results
    - If a field is selected, it will be displayed as a column in the list view
        - The list of results themselves do not change
    - If an event was found and displayed due to the search criteria, but lacks a field that has been selected, then it won't have a value for the new
      column
- Interesting fields are additional fields that have been extracted from the search results
    - Interesting Fields are fields that appear in at least 20% of the events
## Patterns
- The patterns tab shows the most common patterns among the set of events that were returned by the search
## Statistics
- The statistics tab only populates when certain search keywords (i.e. "transforming commands") are used
## Visualization
- The visualization tab only populates when certain search keywords (i.e. "transforming commands") are used
# Useful searches
- Performing a useful search depends on a minimal understanding of what the fields and values of events mean
    - I can have this understanding beforehand, or I can 
## Examples
- See all successful purchases from buttercupgames: `sourcetype=access_* status=200 action=purchase`
- See all unsuccessful purchases from buttercupgames: `sourcetype=access_* status!=200 action=purchase`