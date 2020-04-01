- https://www.splunk.com/en_us/blog/tips-and-tricks/what-are-splunk-apps-and-add-ons.html - app vs. add-on, also options for creating a app UI (useful)
- https://answers.splunk.com/answers/477/what-is-the-difference-between-an-add-on-and-an-app.html - app vs. add-on
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Troubleshooting/Abouttheplatforminstrumentationframework - introspection generator add-on
# What is an add-on?
- An add-on is a single component that can be reused across a variety of use-cases
- It does not contain any navigable user interface
- It cannot be opened from within Splunk Web
- Examples of add-ons:
  - A custom search command
  - A modular input
  - A data model definition
  - A custom rest endpoint
  - Custom field extractions, sourcetype defintions, or macros
  - Resuable JS visualizations, such as something with D3
## Why is it different from an app?
- An app is meant for a single use-case
- An app bundles more functionality than an add-on
  - It will have a user interface
  - It could have looksups, tags, event types, saved searches, etc.
- It can be opened with Splunk Web 
# Existing add-ons
## Introspection generator add-on
- Enables Splunk to write log files that report on system resource utilization and assist with troubleshooting Splunk Enterprise performance issues