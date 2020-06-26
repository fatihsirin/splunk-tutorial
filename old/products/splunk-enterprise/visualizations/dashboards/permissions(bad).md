
# Introduction
- The menu to change the permissions of any objects is the same and is straightforward to use. If things aren't working like they should, I messed up
  my Splunk installation somehow
- In order for a dashboard to be useful to anyone who isn't the owner, the proper permissions need to be set on the dashboard *and* every component
  within the dashboard
# High-level permissions
## Reports
- x
# Low level permissions
## Field aliases
- Even though the permissions on a field alias should make it visible to and usable by any user, only the owner can see and use the field alias. What
  gives? Do I need to reinstall Splunk? Let me try creating a new field alias first to see what happens ... same result. The new alias is not visible
  to a regular user. What if I temporarily upgrade a user to be an admin, what is the result? Same result! Even the new admin cannot use the field
  alias! 
  - After reinstalling splunk, everything works as expected. There are no hidden field alias permissions beyond what's presented. There is no hidden role
    that controls whether or not field aliases are shared
  - Protip: don't muck around in the configuration files. Build an app, then export and import it properly
  

3. Saved searches
- SCADA log: sourcetype="scada" | sort _time, line_id | table timestamp, line_id, power, origin_voltage, destination_voltage
- Timechart: sourcetype="scada" | timechart span=15m first(power) BY line_id
- OMS log: sourcetype="oms" | sort _time | table issue_created, issue_resolved, line_id, outage_cause, bus_affected, amount
- Map visualization: sourcetype="oms" | lookup bus-lookup bus_id as bus_affected OUTPUT latitude as lat, longitude as lon | geostats latfield=lat
  longfield=lon count
 