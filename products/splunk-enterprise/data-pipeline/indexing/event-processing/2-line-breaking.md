- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Configureeventlinebreaking
# These notes need to be reviewed
# Background
- Most events consists of a single line of characters, terminated by a newline
    - Splunk handles these correctly by virtue of the default configuration
- However, some events consists of multiple lines
    - Splunk handles most mult-line events correctly by default *by virtue of the existing configuration settings*, not some magical power
# Default event boundary detection settings
## LINE_BREAKER regular expression
- Splunk first always uses the regular expression specified by the `LINE_BREAKER` attribute to break the incoming stream of bytes into separate lines 
- By default, the `LINE_BREAKER` attribute is a sequence of newlines and carriage returns
    - By default, the regex is `([\r\n]+)`
        - Yes the capture group is required
- Often, this attribute is sufficient on its own
    - This is good because this step is performant as compared to using the settings associated with `SHOULD_LINEMERGE = true`
## SHOULD_LINEMERGE and associated settings
- Splunk optionally can recombine the lines that were split by `LINE_BREAKER` by using the settings associated with `SHOULD_LINEMERGE`
    - If the `LINE_BREAKER` attribute isn't sufficient, then I need these settings
- Setting `SHOULD_LINEMERGE = true` tells Splunk that after the incoming byte stream has been split into separate lines, some of those lines need to
  be recombined to form true, logical multi-line events via some other settings
    - By default, `SHOULD_LINEMERGE = true`
    - This recombining behavior is why using these setting is much slower than using `LINE_BREAKER` alone
# Changing the event boundary detection settings
- If the default Splunk configuration isn't generating multi-line events correctly, then the settings in `$SPLUNK_HOME/etc/system/local/props.conf`
  need to be configured (NOT SURE ABOUT THIS)
    - Save the file and restart Splunk Enterprise to apply the changes
## Event boundary detection strategies
### Use only LINE_BREAKER to directly split events
- This is the preferred strategy because it is more performant
- However, it could entail writing a very complex, difficult regex, which is why the second strategy exists at all
### Use SHOULD_LINEMERGE to recombine events
- Although this strategy is slower, it is much easier to work with
# Event boundary detection settings
- See source for exact details
## TRUNCATE
- Set the maximum number of bytes that compose a line
- I believe this setting will only take affect if the byte stream isn't split as described in the above sections
