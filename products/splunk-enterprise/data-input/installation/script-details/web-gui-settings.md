- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Modifyinputsettings - app context setting
- https://answers.splunk.com/answers/240615/how-would-i-know-what-applicatoin-context-to-choos.html - app context setting
# Introduction
- These settings are all configured through the Splunk Web GUI when a script is newly added as a data input
- It is possible to modify a data input after it has been addded, but for scripts it wasn't apparent how to modify all of the settings after creation
  time
  - E.g. I didn't see how to edit the `Command` setting post-creation
# Settings
## Command
- The `Command` setting is how Splunk invokes the script
- Splunk Web performs validation to ensure that `Command` is a valid command, according to whatever directory the command is being resolved from
### $SPLUNK_HOME
- `$SPLUNK_HOME` is set inside of whatever process Splunk is running inside of, even if I don't explicitly set this environment variable in any of my shells
    - On macOS, `$SPLUNK_HOME` is set to `/Applications/Splunk`
### Default directory resolution
- On macOS, if the `Command` string does not include any path components, then Splunk thinks that whatever command is input is relative to the
  `/Applications/Splunk/bin/scripts/` directory
### Custom directory resolution
- The directory from which the command is resolved can be customized by entering a path for `Command`
    - E.g. Entering `$SPLUNK_HOME/etc/apps/<my app>/bin/<my script>` will make Splunk search the app's directory for the script instead of the global
      Splunk script directory
        - Entering the above command is equivalent to entering `etc/apps/<my app>/bin/<my script>`
        - Entering the above command is *not* equivalent to entering `/etc/apps/<my app>/bin/<my script>`, which isn't allowed
## App context
- The chosen app context for the newly added script input does not affect searching in any way (according to the forum)
  - I don't think this is necessarily true. Remember that Splunk resolves configuration files with a precedence ordering. If two apps referenced the
    same script, but had different configurations in their `inputs.conf` files, then the app context would matter, right? Because the expected
    configuration wouldn't be applied for one of the apps?
      - Actually maybe not. See the `app-external-script.md` notes. I need to understand precedence resolution *between* apps
- See the `application-context.md` notes
# Run a validation search on the newly added script
- To make sure that Splunk really is getting data from the script, I should run an initial search for data that is supposed to come out of the script
    - E.g. `source="blah_source" host="Austins-MacBook-Air.local" sourcetype="generic_single_line"`
## Troubleshooting
- Splunk does not raise any errors if the script throws an exception or fails to run entirely    
    - Ensure that the file runs on its own
- `$SPLUNK_HOME` does *not* need to be set in any user shell