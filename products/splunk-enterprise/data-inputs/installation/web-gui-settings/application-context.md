- https://docs.splunk.com/Documentation/Splunk/8.0.2/Data/Modifyinputsettings - application context introduction
# What is the app context?
- When a new data input is added to Splunk Enterprise, the "app context" setting (a.k.a. the "application context" setting) must be set
  - This setting is for the data input itself, not for any app
    - The app context of the data input *is* just an app that exists within the Splunk Enterprise installation
- The selected app context determines:
  - Which app will stores the data input's configuration
    - E.g. which app will create or modify its own `inputs.conf` file(s) to include the new data input
    - The only reason I can think of that this matters is that it makes it easier to manage Splunk's crazy amount of configuration files
    - It seems to me that any custom data input (e.g. a script) would only want to be associated with its own custom app because when a third-party
      installs the app, they would expect the entire end-to-end workflow to exist within that app 
      - E.g. it wouldn't make sense to store the configuration for `my_scripts.py` inside of the `Searching & Reporting` app because then the user
        wouldn't get those configuration files when they installed the app!
      - I suppose Splunk allows users to customize the app context because you never know what configuration a user will need. However, I should
        always choose the app context to be the custom app that will be consuming the custom input
# Existing app contexts
- The following sections are brief descriptions of the applications that Splunk Enterprise comes with
- These descriptions help me understand when I would want to choose a particular app to be an app context
- The term "application" is confusing because some of these applications seem more like pieces of functionality that could be incorporated into any
  app
  - E.g. perhaps I want to configure a webhook alert action to work with SCADA data. Is the "Webhook Alert Action" really its own conceptual app? Some
    Splunk "apps" are reusable units of functionality while others are real applications with their own dedicated front-end GUIs
- I can't select an add-on to be an app context, which simplifies these notes
# Directories within apps/ that are visible as app contexts
- I can't think of any reason to ever select some of these apps to be an app context
  - This is unfortunate because the dropdown of selectable app contexts is alphabetized, which makes it seem like some of these would be a good choice
    to be a data input's app context
## appsbrowser/ (Apps Browser)
- Displays other Splunk apps that can be installed
## alert_logevent/ (Log Event Alert Action)
- Enables writing a log entry when an alert is triggerred
  - A Splunk alert uses a saved Splunk search to look for events in real-time or on a schedule. When the search meets specific conditions, the alert is
    triggerred. When the alert triggers, I can configure an alert actions to occur, such as a log event or a webhook event
## alert_webhook/ (Webhook Alert Action)
- Enables submitting an HTTP POST request to a URL when an alert is triggerred
  - E.g. post in a forum when an alert is triggerred
## launcher/ (Home)
- Determines which apps to show in the welcome tab
## search/ (Searching & Reporting)
- Provides the default interface for searching basically any type of data
- I shouldn't put any additional configuration in here if I can help it
  - If I do, it appears to go into the local/ directory anyway, so it's not a big deal
## splunk_archiver/ (Splunk Archiver App)
- Provides Splunk bucket archiving functionality to Hadoop compatible filesystems
## splunk_gdi/ (Splunk Get Data In)
- Provides the nice web page workflow that is displayed when I click "Add Data" on the homepage! 
## splunk_httpinput/ (splunk_httpinput)
- Contains HTTP configuration settings
- Not sure why this is selectable as an app context
## splunk_instrumentation (Instrumentation)
- Connects the hosting Splunk instance to Splunk's usage data collection services
- Not sure why this is selectable as an app context
## splunk_internal_metrics (Clones Internal Metrics Into Metrics Index)
- Clones, converts and stores various internal splunk logs into _metrics index
- Not sure why this is selectable as an app context
## splunk_metrics_workspace (Splunk Analytics Workspace) 
- Compare trends in time series
## splunk_monitoring_console (Monitoring Console)
- Provides insight into my Splunk deployment
# Directories within the apps/ that aren't visible as app contexts
- Some of these "apps" aren't visible because they aren't really apps
  - Splunk stores a bunch of app-like functional units in the apps directory, even though these directories aren't real apps
  - These "apps" must provide core functionality of the Splunk platform that the user should never need be aware of
- Other "apps" aren't visible because they are disabled
## learned/
- Stores automatically generated sourcetypes
## legacy/
- Disabled
- Must contain old depreciated settings?
## sample_app/
- Disabled
- Must provide an example template for writing a real app?
## SplunkForwarder/
- Disabled
- Maybe it is used if I actually use forwarders?
## SplunkLightForwarder/
- Disabled
- Maybe it is used if I actually use forwarders?
## user-prefs/
- I'm never supposed to edit files in here. Instead, copy the stanza that I want from these files into the files in some local/ directory and make changes there