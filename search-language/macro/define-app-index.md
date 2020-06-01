- https://docs.splunk.com/Documentation/Splunk/8.0.3/Knowledge/Usesearchmacros - macro documentation
- https://www.splunk.com/en_us/blog/tips-and-tricks/app-design-patterns-creating-indexes.html - example of bundling a macro with an app to make it
  easier for an admin to use a custom index with the app
# Examples
## Define an app index
### File
#### /Users/austinchang/repositories/aifbdp-splunk-apps/untopchan/default/macros.conf
```
[untopchan_index_macro]
definition = index=electrical_utility_data
iseval = 0
```
### Search
```
`untopchan_index_macro` sourcetype=realtime_scada ...
```
- A macro must be surrounded by backticks to be expanded
# Purpose
- A macro is just a reusable SPL string
## Use a macro to search a specific index
- It is common for app developers to use isolated indexes to store data that is supposed to be consumed by their particular app
- It is bad practice to include specific index names in the saved searches compose dashboards/reports/etc.
  - E.g. if the developer was using the "cat" index to store information about cats, but some Splunk admin only had an index named "animals" for the
    cat data, the Splunk admin would have to manually create new copies of the dashboards/reports/etc that used the string "animals" instead of "cat".
      - The poor Splunk admin would have to redo this process everytime the developer updated the app!
- Instead of doing that, the developer should use a macro within the saved searches that compose the app's dashboards/reports/etc.
  - This macro will be bundled with the app. Now instead of having to change every knowledge object that uses the macro, a Splunk admin can change the
    macro in one place and that change will be permanent across app updates
  - The macro provides a layer of abstraction to decouple the developer's specific index name preferences from the app
    - The Splunk admin can use the macro or leave it blank as ""
# Splunk web management
- Macros can be accessed via Settings > Advanced search