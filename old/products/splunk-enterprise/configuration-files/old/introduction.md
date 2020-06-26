- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Aboutconfigurationfiles - starting page for configuration files
- https://answers.splunk.com/answers/37729/deployment-config-files-in-local-or-default.html - local vs. default
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationfilechangesthatrequirerestart - when restart is needed
- https://answers.splunk.com/answers/593810/are-there-any-recommended-settings-for-file-permis.html - file permissions
# Configuration file purposes
- Configuration files can control:
  - System settings
  - Authentication and authorization information
  - Index-related settings
  - Deployment and cluster configurations
  - Knowledge objects and saved searches
- The more I read, the more convinced I am that the "magic" of Splunk is just a huge bunch of configuration files
# Modifying configuration files
- Whenever a configuration file is modified via Splunk Web or directly, a copy of the default file is created in a specific directory (e.g.
  `$SPLUNK_HOME/etc/system/local`) *if* a copy does not already exist, and the changes are written to that copy
- Some configuration modifications cannot be done via Splunk Web
- The new configuration file copy should not copy anything from the default file. This must mean that Splunk treats all settings in the copied file as
  overrides
- Remember that a configuration file only affects the data pipeline if it is set on the Splunk component that is handling the associated phase!
  - Read the data-pipeline introduction.md notes 
# null value
- Delete the key entirely for an attribute to set the value of the attribute to null. Splunk will literally use the null value if it has highest
  precedence
  - E.g. `forwardedindex.0.whitelist = `
    - Now no machine matches the whitelist? IDK
- I don't yet know why this would be a good idea. Maybe to match nothing, explicitly?
# File permissions
- Splunk recommends:
  - 644 for all files outside of /bin
    - That's read/write for owner and read for group and world
  - 755 for all directories and files in /bin
    - That's read/write/execute for owner and read/execute for group and world
# Encoding
- Splunk works with configuration files encoded in ASCII/UTF-8
  - If a platform (e.g. Windows, yes actually Windows) doesn't have UTF-8 as the default character set, write the configuration files in the default
    character set of the OS
# Restarting to apply changes
- See source
- Just read the source. It's very complicated. I don't want to write something wrong here