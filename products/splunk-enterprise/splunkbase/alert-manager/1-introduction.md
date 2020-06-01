- https://github.com/alertmanager/alert_manager/tree/release/v2.2 - alert manager latest release
- http://docs.alertmanager.info/en/latest/ - alert manager documentation
# Q & A
- What is the difference between `<app>/appserver/static/` and `<app>/static/`?
  - IDK
# How it works
- I cannot create alerts from within the Alert Manager
  - Alert Manager simply intercepts and records regular Splunk alerts? It would seem so, but my regular untopchan alerts are firing and nothing is
    happening... Ah! I just need to add the alert manager action to the regular Splunk alert!