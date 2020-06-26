- https://docs.splunk.com/Documentation/Splunk/latest/Updating/Useforwardermanagementtomanageapps
# Process
- First, uninstall the app through the deployment server interface
  - This _does_ remove the deployment app from the apps/ directory of every client that it was deployed to
  - This does _not_ remove the deployment app from the deployment server file system itself
- Second (optionally), go into the deployment server instance and remove the apps from the file system