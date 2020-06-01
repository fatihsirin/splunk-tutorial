- https://dev.splunk.com/enterprise/docs/developapps/createapps/createsplunkapp - app directory structure
# Directory structure
```
<app name>/
  appserver/
    static/
      - Contains CSS, JS extensions, and icon files. All static assets for custom web pages go in here
  bin/
  default/
  local/
  lookups/
  metadata/
  static/
    - Contains icon files to represent the app in Splunk web
```

# Refresh rules
- Restarting Splunk Enterprise always clears cached static assets on the server
  - This is time-consuming and annoying. Instead, just hit `http://<host:port>/debug/refresh`

# Making a custom web page within Splunk Web
- Create a Simple XML dashboard
  - Is this necessary? What does this do?
    - I just literally went into my app and clicked "Create New Dashbaord" and got a super basic dashboard
- Now I'm editing the source code in Splunk Web
  - I pasted in some custom HTML
- I put two custom JS files into appserver/static/
- I restarted Splunk Enterprise
- I'm supposed to use `simplesplunkview.js` as a base class to inherit from and override. That class does the following:
  - Gets data from Splunk and returns a handle to it
  - Create a custom view and puts data into it
  - Renders and updates the view
  - I'll need to override at minimum the following methods:
    - formatData():
    - createView():
    - updateView():
- 