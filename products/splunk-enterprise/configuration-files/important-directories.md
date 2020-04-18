- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Configurationfiledirectories
- https://answers.splunk.com/answers/37729/deployment-config-files-in-local-or-default.html
# Important directories
- These directories are listed in descending order of precedence:
  - `$SPLUNK_HOME/etc/users`: user-specific configuration
  - `$SPLUNK_HOME/etc/apps/<app_name>/[local|default]`: app-specific configuration
    - /local contains end-user app-specific configuration
    - /default contains app-specific default configuration
  - `$SPLUNK_HOME/etc/system/local`: app-wide configuration
  - `$SPLUNK_HOME/etc/system/default/*`: base default configuration
    - The files in this directory are never supposed to be modified, except when Splunk installs or updates itself
    - If a configuration setting isn't found in a file with higher precedence, then the setting within a file in this directory is used
- Keep in mind that some configuration files will never exist in certain directories
    - E.g. a configuration file that exists exclusively in the global context will never exist in the `$SPLUNK_HOME/etc/users` directory
# `/local` vs. `/default` for an app
- Within an app's own configuration directory, `/default` has lower precedence than `/local`
- If I'm developing an app, then I should store the final configuration of the app in `/default`
    - This is because `/local` is meant to be used by a final end-user, so it doesn't make sense for an app's default settings to be set there
- If I'm an end user, then I should store my configuration options in `/local`
    - I could also store them in `/default` if I wanted other users to have my configuration, but think carefully before doing that
# Find example configuration files
- The `$SPLUNK_HOME/etc/system/README` directory contains the most detailed examples of configuration files
    - A `*.spec` file demonstrates the syntax of the configuration file
    - A `*.example` file demonstrates real-world usage of the file