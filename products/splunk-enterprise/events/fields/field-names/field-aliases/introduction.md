- https://docs.splunk.com/Documentation/Splunk/latest/Knowledge/Addaliasestofields - introduction to field aliases
# Introduction
- When field aliases appear as interesting/selected fields in the search results, by default the original fields that they reference will also appear
  in the search results
- Field aliases are saved in `props.conf`
- Field aliases should be grouped under a sourcetype! Don't delete a seemingly useless sourcetype because it might be grouping one or more field
  aliases!
# Field aliases can change behavior
- TLDR: never select "overwrite field values" when creating a field alias because I can't currently think of a scenario where it would be a good idea
- The concept of a field alias is intuitive. What's not intuitive is that field aliases can affect the search results in unintended ways depending on
  what the search returns
- In the following notes, consider the case where an original field, "src", has been given the alias "dst"
## Events contain both alias and original field
- What happens when the events returned by a search contain both "src" and "dst", as in there are two real, unassociated, original fields "src" and
  "dst"? It depends on whether "overwrite field values" was selected when the field alias was created
- Ideally this situation would never occur because I understand what data I'm ingesting
### Overwrite field values was selected
- The search head replaces the values associated with the field "dst" with the values associated with the field "src", because "dst" was defined to
  be an alias of "src"
  - I think this is saying that "dst" and "src" will still be present in the events, but that the real values of "dst" will be lost and overwritten
    with the values of "src"
  - The net result is that all the real values of "dst" are lost
### Overwrite field values was not selected
- The values associated with the field "dst" are unchanged
  - This is what I would want to happen in this situation
## Events only contain the alias field
- What happens when events returned by a search only contain "dst," as in there is a real, unassoicatied, original field "dst" present in the raw data
  that never had anything to do with "src"? It depends on whether "overwrite fields values" was selected when the field alias was created
### Overwrite field values was selected
- The search head removes "dst" from the events because "dst" is an alias of a field that is not present
  - This would be disasterous. I would never even realize that I was missing a real field
### Overwrite field values was not selected
- The field "dst" remains as-is
  - This is what I would want to happen