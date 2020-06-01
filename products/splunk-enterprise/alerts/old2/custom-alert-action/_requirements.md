# Introduction
- Alerts are way too complicated for me to simply read the documentation. I need to think of specific use cases first
- What looks good in a demo?
  - Adding the alert to the triggered alerts page is very basic and not very impressive. It would be nice to see some sort of interesting custom view.
    This actually sounds like fun
# Custom alert objectives
- The default Splunk actions for alerts are not sufficient for an interesting demo
  - Writing to a log file or displaying an entry in the "Triggered Alerts" Splunk Web page is not very impressive
## What information do I want to show in my custom alert?
  - Alert when Power_MVA == 0
  - Show the full realtime_scada event with Power_MVA == 0
  - Should there be a log level associated with the event?
    - A typical approach is to have 3 levels: INFO, WARNING, and CRITICAL
  - If there is a realtime_oms event that aligns with the realtime_scada event, then there should be an "expected" indicator
  - If there is no realtime_oms event that aligns with the realtime_scada event, then there should be an "unexpected" indicator
  - Can I display a list of remedial actions or personnel that should be contacted?
  - Can I display a visual map of the event?
    - Not all alert events would have a visual component, would they? I guess they would since this whole analysis is centered on physical
      transmission infrastructure
  - I can show a snapshot of the events leading up to and after the alert event
  - Could I feed the realtime_scada event (and other events) into some sort of script that would determine if the event was unexpected? Now I'm
    thinking about ML
  - I need to display some sort of text message to go with the realtime_scada event. I shouldn't assume that anyone can understand why a Splunk
    event is significant
## How am I going to store the alert information?
- x
## How am I going to display that information?
- x
# Development approach
- I should play around with the existing Splunk apps to see what they do
# Other considerations
## Splunk alert display options
- Display the alert in the triggered alerts page
- Create a custom index that holds alert events, then have a dashboard panel that searches that index and displays the results
## Useful existing apps
- http://docs.alertmanager.info/en/latest/ : this Splunkbase app is a general-purpose alert add-on. It adds better visualizations and other tools to
  deal with alerts. Very cool! We might be able to just install this and use it instead of me making something identical. Note the detailed
  installation instructions provided in the link. If I create an app, I might want to emulate that documentation
  - There are literally 400 similar apps on Splunkbase!