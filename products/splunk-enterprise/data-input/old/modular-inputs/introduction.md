- https://docs.splunk.com/Documentation/Splunk/8.0.2/AdvancedDev/ModInputsIntro - high-level introduction
# What is a modular input?
- A modular input is a custom input capability that users can select and configure, just like any other Splunk input
    - A modular input is a Splunk add-on, similar to an app or add-on
- The functionality provided by a modular input might be overkill for really simple scripts, but I believe that it's best to use the modular input API
  to write scripts in order to conform to Splunk best practices
    - It seems like a modular input is a way to create a wrapper around a data source that might be cumbersome to consume on its own through the
      traditional Splunk file input, UDP/TCP input, or script input configurations
# Modular input features
- See source for a nice table
## Modular input vs. script
- See source for a nice table
- Basically, a modular input abstracts a lot of manual implementation and configuration out of the script and puts it into Splunk