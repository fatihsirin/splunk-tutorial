# Conditional functions are not for filtering events
- A conditional function *must* return a value. It's a function after all! Therefore, it cannot be used to filter out events
  - Use the `where` clause for filtering events
    - E.g. `sourcetype="scada" | where origin_bus=4 or destination_bus=4 | eval the_voltage = if(origin_bus=4, origin_voltage, destination_voltage)`