- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Wheretofindtheconfigurationfiles
# Introduction
- The Splunk documentation says that I should never need to care about how the directory names of different apps affect the order of
  evaluation (i.e. precedence) of configuration files that set the same setting to different values (i.e. there's a conflict between configuration
  files)
  - Since this is the case, don't spend a lot of time on these notes unless I encounter a nasty bug in a Splunk configuration. Just make each app
    manage its own configuration and let Splunk do its thing when it merges configuration files
- I should still care about how the configuration files are combined in the global vs. app/user contexts, but that discussion is completely separate
  from the idea here about inter-app configuration conflict