- https://www.splunk.com/en_us/blog/tips-and-tricks/app-design-patterns-creating-indexes.html - packaging an app with index definitition is an
  anti-pattern
- https://answers.splunk.com/answers/716160/how-do-you-package-an-app-with-an-index-or-data.html - not very helpful but its a start on packaging data
  with an app
- https://www.splunk.com/en_us/blog/tips-and-tricks/defining-indexes-for-release-with-your-app.html - thoughts on "including" an index with an app
# Introduction
- Packaging an app with index definitions is an anti-pattern in a real Splunk app
  - This argument is predicated on the fact that the end-user will be or have access to a Splunk administrator who understands how to properly
    configure an app that was properly developed
- However, I still believe including index definitions is valuable for the purposes of demoing and prototyping
# Create new index when app is installed

- 