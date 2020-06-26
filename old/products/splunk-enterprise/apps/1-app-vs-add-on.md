- https://www.splunk.com/en_us/blog/tips-and-tricks/what-are-splunk-apps-and-add-ons.html - app vs. add-on, also options for creating a app UI (useful)
- https://answers.splunk.com/answers/477/what-is-the-difference-between-an-add-on-and-an-app.html - app vs. add-on
- https://answers.splunk.com/answers/463299/what-is-the-difference-between-apps-add-ons-and-ta.html - What does "TA" mean?
# Usage intent differences
- An app is intended to be a complete solution for a single use case
  - It will have a user interface
  - It could have looksups, tags, event types, saved searches, etc.
  - It can be opened with Splunk Web 
- An add-on is intended to be a modular component that can be used for a variety of use cases
  - Examples of add-ons:
    - A custom search command
    - A modular input
    - A data model definition
    - A custom rest endpoint
    - Custom field extractions, sourcetype defintions, or macros
    - Resuable JS visualizations, such as something with D3
  - An add-on does not:
    - Contain any navigable user interface
    - Have a way to be accessed from within Splunk Web
# File differences
- Splunk apps and add-ons both use many of the same configuration files (e.g. `app.conf`)
- However, due to their smaller, modular nature, add-ons tend to contain a subset of the *.conf files that would typically be found in an app
  - E.g. an add-on typically shouldn't contain any files that would be used to visually display a page in Splunk web, but typically would contain a
    `props.conf` to perform index-time or search-time field extraction 
    - E.g. the "alert manager" app and the "TA-alert_manager" add-on both have the same directory structure. However, they contain different
      configuration files. The app has `inputs.conf` and and `appserver` directory while the add-on has `props.conf` and `eventtypes.conf`
# Caveats
- There is no hard line between add-ons and apps
  - E.g. an app can be partitioned into different node workloads (e.g. search heads, indexers, and forwarders) by a Splunk admin. However, the files
    that are partitioned into the indexer and forwarder workloads _could_ be bundled in a separate add-on by the developer
    - The Alert Manager app takes this approach
- The packaging of an app vs. an app and add-on vs. just an add-on is really up to the developer based on how they envision their code will be used.
  Add to this fact that an admin can partition any app or add-on according to how they want to structure their topology. The lines between "apps",
  "add-ons", partitioned "sub-apps", and partitioned "sub-add-ons" get blurry fast
# What is a technology add-on (TA)?
- Apparently, the only way a TA is different from a normal add-on is that a TA is guaranteed to conform to CIM?