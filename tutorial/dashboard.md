- https://docs.splunk.com/Documentation/Splunk/8.0.2/SearchTutorial/Aboutdashboards
# Introduction
- A dashboard is made of panels, which are usually connected to reports
- After a search visualization is created, or a report has been saved, it can be added to a new or existing dashboard
- The dashboard editor is useful for adding saved reports to a dashboard
- Directly editing the dashboard XML allows me to customize features that I cannot customize from within the dashboard editor
# Create a dashboard
## Connecting input controls and panels
- Input controls that are added to a dashboard are, by default, independent from any panels on the dashboard
    - An input control and a panel must be connected explicitly
        - Multiple panels can be connected to the same input control
        - Remember to click "Save" to finish connecting a dashboard input and a panel
### Time Range Picker
- If a panel is an ad hoc search, it can be connected to shared Time Range Picker
    - If a panel is a report, it cannot be connected to a shared Time Range Picker
        - Reports can be scheduled to run at specified intervals
