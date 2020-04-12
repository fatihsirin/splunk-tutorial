- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ModInputsBasicExample - basic modular input example
- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ModInputsScripts - more details on file locations
# Modular input components
- In the following sections, `<my app>` is equivalent to `<my modular input name>`
## Required components
- The actual script that underlies the modular input usually goes in the `$SPLUNK_HOME/etc/apps/<my app>/bin` directory
    - I can also place the script in an architecture-specific directory of a Splunk Enterprise instance (see source)
- The modular input default scheme defined in `inputs.conf.spec` must go in the `$SPLUNK_HOME/etc/apps/<modular input name>/README` directory
## Optional components
- The `app.conf` file that configures the modular input as an add-on must go in the `$SPLUNK_HOME/etc/apps/<modular input name>/default` directory
- The `default.meta` file that sets the script's sharing permissions must go in the `$SPLUNK_HOME/etc/apps/<modular input name>/metadata` directory