- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Wheretofindtheconfigurationfiles - cross-file precedence
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Attributeprecedencewithinafile - within-file precedence
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Specifyinputpathswithwildcards - ellipsis pattern in Splunk
- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Propsconf - props.conf
# These notes probably need their own directory
# Introduction
- Splunk determines the precedence of a configuration file, and its contained settings, via its directory location
    - Splunk also determines precedence based on somewhat complex lexicographical sorting (see source)
- If two configuration files set the same setting, the file with higher precedence wins
- Configuration files can exist in the global context or in the context of the current user and app
# Configuration file context
- A configuration file can exist in a global context or in an app/user context
    - A few configuration files can exist in both contexts (e.g. `props.conf`, `transforms.confg`, and a few settings in `limits.conf`, which is
      mostly global)
- Generally, configuration files that affect data input, indexing, or deployment activities are global
- Generally, configuration files that affect search activities are app/user
- See the source for a list of configuration files and their contexts
## Global context
- If a configuration file exists in the global context, descending directory precedence order is:
    - System local: `$SPLUNK_HOME/etc/system/local/*`
    - App local: `$SPLUNK_HOME/etc/apps/A/local/* ... $SPLUNK_HOME/etc/apps/z/local/*`
    - App default: `$SPLUNK_HOME/etc/apps/A/default/* ... $SPLUNK_HOME/etc/apps/z/default/*`
    - System default: `$SPLUNK_HOME/etc/system/default/*`
- E.g. `inputs.conf` can exist in any of the above directories, but the configuration in the system local directory take precedence over those found
  in any app directory (weird I know, but it's true))
## App/user context
- If a configuration file exists in the app/user context, descending directory precedence order is:
    - Current user: `$SPLUNK_HOME/etc/users/*`
    - Current app local: `$SPLUNK_HOME/etc/apps/<Current_running_app>/local/*`
    - Current app default: `$SPLUNK_HOME/etc/apps/<Current_running_app>/default/*`
    - Exported settings of other apps (local, then default): `$SPLUNK_HOME/etc/apps/z/local/*, $SPLUNK_HOME/etc/apps/z/default/*, ... $SPLUNK_HOME/etc/apps/A/local/*, $SPLUNK_HOME/etc/apps/A/default/*`
    - System local: `$SPLUNK_HOME/etc/system/local/*`
    - System default: `$SPLUNK_HOME/etc/system/default/*`
# Precedence within a single file
- Just play around with the settings to see how this works because the references aren't good enough
## Implicit stanza priority
- Stanzas that literally match a string take precedence over regex pattern-matching stanzas
    - E.g. `???`
    - Pattern-matching stanzas have an implicit priority of 0
    - Literal-matching stanzas have an implicit priority of 100
## Manually set stanza priority
- I can override the default stanza precedence ordering via the `priority` key
    - A higher integer means higher precedence
```
[source::...a...]
sourcetype = a
priority = 5

[source::...z...]
sourcetype = z
priority = 10
```
- Normally, the settings under the `[source::...a...]` stanza would take precedence over the other stanza due to lexicographical ordering, but the
  explicit priority key changes this