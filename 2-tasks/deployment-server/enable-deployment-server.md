- In addition to the deployment server not being explicitly disabled, there must be at least one directory within the `deployment-apps/` directory for
  the deployment server UI to be activated
  - If there is at least one directory with `deployment-apps/`, then within Splunk Web Settings > Forwarder management will show something, but
    Settings > Server settings > Deployment client won't show anything
  - Settings > Server settings > Deployment client won't event show something if an app is distributed to a client
    - What is this screen for?