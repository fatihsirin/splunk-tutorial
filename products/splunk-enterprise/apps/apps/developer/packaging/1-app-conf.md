# Examples
## Minimum app.conf contents
### File
#### /Applications/Splunk/etc/apps/untopchan/default/app.conf
```
[launcher]
version = 0.0.1
[install]
build = 1
[ui]
label = Unexpected Topology Changes
```
- These are the minimum attributes that _should_ be in `app.conf`. I'm going to say they're the minium required attributes
  - `slim` doesn't raise any errors if `app.conf` is entirely empty. I can't tell if this is because I'm using an old version of `slim` or what
## Good, but not complete, app.conf contents
### File
#### /Applications/Splunk/etc/apps/untopchan/default/app.conf
```
#
# Splunk app configuration file
#

# This section doesn't appear to be mandatory
#[author=Austin]
#email = asc1995@gmail.com
#company = NRECA

# If this stanza is included (it's not mandatory), "name" and "version" must be included
[id]
# This is the name of the development team. If this attribute is included, then the following equations must be true:
# - 1) [id].group + [id].name == [package].id
# - 2) [id].group + [id].name == <the name of the app folder>
group = Goron City
# The name of this Splunk app. Must match [package].id. Therefore, this attribute is redundant
name = untopchan
# The version of this Splunk app. Must match [launcher].version. Therefore, this attribute is redundant
version = 0.0.1

[launcher]
# (Required) Must match the version specified in app.manifest
version = 0.0.1 
# Splunkbase uses the description to validate the app package
description = App to identify unexpected topology changes in an electrical grid 
# Author's email address (or splunk.com account if deploying to Splunkbase)
author = asc1995@gmail.com 

# This section define upgrade-related metadata
[package]
# This setting is only needed for apps uploaded to Splunkbase, but putting it here stops the annoying packaging warning
id = untopchan 
# Don't check Splunkbase for updates
check_for_updates = false 
# Don't show an upgrade notification (Older versions of slim complain that this is an unrecognized configuration setting)
#show_upgrade_notification = false 

[install]
# If an app is disabled in Splunk, it's configuration is ignored 
state = enabled 
# Indicates whether or not the user has run the one-time custom setup workflow for the app
is_configured = 0 
# (Required) Every release must change the "version" and "build" settings. Technically, this number should be incremented when static assets in appserver/static are changed
build = 1 

[ui]
# Apps that contain a user interface should be visible
is_visible = 1 
# I want my app to appear in the global drop down
show_in_nav = 1 
# (Required) This is the name of the app displayed in Splunk Web
label = Unexpected Topology Changes 
```
# Purpose
- The configuration settings in `app.conf` do many things:
  - Affect the visual display of the app
  - Can trigger refreshment of app static assets
  - Store metadata about the app
  - etc.
- `app.conf` is mainly read from by the user after they install the app. However, the user also writes to it once after completeing the one-time app
  setup (if provided)
# Editing workflow
- `app.conf` must be manually edited
- Be sure that `app.conf` is stored in `default/`, not `local/`
- Comments can __never__ go on the same line as actual configuration
  - My editor makes it look like including comments on the same line is okay, but it's not! This goes for all configuration files!
# Files
- The `app.conf` file stores the configuration of the app