- https://dev.splunk.com/enterprise/docs/releaseapps/ - publish an app
# Introduction
- Developers can submit apps to a public Splunk repository of apps
- Other Splunk users can then download those apps to incorporate into their Splunk Enterprise workflows
- Thus, I'm not sure that we will want to make our app available via some "deploy.sh" script that users can run. It might be better practice to just
  publish the app
  - This is not true if the work we are doing is classified or something. Then we won't be able to publish it publically