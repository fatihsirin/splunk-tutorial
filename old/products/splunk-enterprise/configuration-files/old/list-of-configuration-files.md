- https://docs.splunk.com/Documentation/Splunk/8.0.2/Admin/Wheretofindtheconfigurationfiles - two lists: one for global and one for app/user context.
  Also props.conf example
- https://docs.splunk.com/Documentation/Splunk/latest/Admin/Listofconfigurationfiles - another list will little summaries for each file
- https://answers.splunk.com/answers/119/what-is-role-of-transforms-conf-vs-props-conf-for-field-extraction.html - props.conf vs transforms.conf
# Introduction
- Read the documentation inside of each example configuration file
# inputs.conf
- Sets up data inputs
## Configure a script
- Configure script parameters:
    - Filesystem location
    - Invocation interval
    - Python version
    - etc. 
# props.conf
- Set indexing property configurations 
- Essentially, I can define a new `<soucetype> | <host> | <source> | <rule> | <delayedrule>` object by putting its name in a stanza, then I can use
  the same set of key-value settings to configure the new object
    - Thus, in these notes it doesn't make sense to distinguish between configuring two objects, such as a source type and a host 
- This file exists in the global and app/user contexts
    - Splunk evaluates props.conf at both index time (global) and search time (apps/user) 
        - Read the configuration-precedence notes to see the exact example on `props.conf` precedence (TLDR: certain settings in `props.conf` are
          evaluated at index time. Index time occurs before search time, so those specific setting values that exist within a `props.conf` file that
          exists within a directory that is evaluated at index time take precedence over any conflicting setting values that exist within a
          `props.conf` file that exists within a directory that is evaluated at search time. Jesus)
# transforms.conf
- Configure regex transformations to perform on data inputs
- Before many (but not all?) settings in this file will have any effect, there must be corresponding settings in `props.conf`
    - E.g. ...
    - At a high level, `props.conf` specifies what rules are applied to any event while `transforms.conf` specifies what those rules actually are (see
      source)
- This file also exists in the global and app/user contexts