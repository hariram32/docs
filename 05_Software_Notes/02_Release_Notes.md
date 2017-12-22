<!-- @@@title:Release Notes@@@ -->

Release Notes - Version 5.88
---

API

* Tasks

 - Enhanced performance on incoming event processing
 - Right-click->execute script was borked in the search results page. We unborked it.
 - Added automatic repair of missing data resulting from end-user disk full.
 - ParserModule performance degradation was a tad overzealous in it's warnings. After a holiday, It's now now much more relaxed.
 - Ensure that command line tools run using sudo do not change file permissions for the logzilla user.

* Bugs
 - RBAC was not RBAC'ing properly for some environments. It does now.
 - Added better escaping for invalid user-created patterns in `/etc/logzilla/rules.d`


Release Notes - Version 5.87
---

API

 - Added better error reporting for invalid rules (such as poor regex patterns)
 - Added ability to set `actionable` or `non-actionable` flags using rules in /etc/logzilla/rules.d
 - Added command line tool `lz5rules performance` which allows performance testing of rules located in /etc/logzilla/rules.d
 - Added ability to import old data streams (previous versions would only accept "real time" data).
 - JSON export of dashboards or triggers containing some unicode characters would fail to export.
 - API Requests should return "Access Denied" rather than a generic "403" error

Release Notes - Version 5.86
---

API

 - Added `lz5stats` command line option to provide a quick summary of current server metrics
 - Removed version dependencies for syslog-ng
 - Moved "Cisco Most Actionable" trigger to the last position so that it fires after other more focused rules.
 

Release Notes - Version 5.85
---

API

* Task
 - Allow `lz5triggers export` to export individual triggers
 - Add Malware IoC's as a tag for individual Malware names
 - Set worker during LogZilla install based on server's available cores
 - Add rewrite for program on malware-ioc's

* Bug
 - Error when asking for malware-iocs rules: 404
 - When install fails, it sometimes doesn't give a reason


Release Notes - Version 5.84
---

FEATURE

* Added LDAP Authentication
* Added `lz5rules` to help users with adding/disabling/re-reading rule files from `/etc/logzilla/rules.d`
* Added ability to set the hour of day in which Autoarchive runs


API

* Task
 - Reduced number of non-useful internal events
 - Average calculations should not include zero's when exporting data
 - Google and yahoo code used in `/api/docs` should be stored locally
 - Moved trigger tracking to internal tags for better performance.
 - Set default for User Tags feature to `enabled`

* Bug
 - UT Source and Dest Ports were showing a `-` as one of the ports
 - Warnings in logzilla.log we're more indicative of an INFO than WARN
 - Autoarchive cleanup was leaving some old files...which wasn't very "clean-y" of it...

UI

* Bug
 - Widgets would display incoming time of events as `in a few seconds` if the user's local system had a poorly sync'd/misconfigured time.


Release Notes - Version 5.83
---

API

* Task

 - Remove repeated trigger id from event TimePoints
 - Convert well-known ports to names and other ports to `dynamic`
 - [Performance] Improve duplication tps sorting
 - Updated rewrite rule for windows events


* Bug

 - Triggered Emails translating some characters to html
 - Fixed Balabit/syslog-ng update bug (their repo crashed)


UI

* Bug

 - Notifications badge wasn't updating count after delete
 - After clicking reset in query bar, pressing `enter` on text search would not trigger search (required actual click)
 - Context-sensitive right click menu (from widgets) was not...contexting.
 - Average Disk Usage Values were 5% off due to OS reserved space
 - Regression Fix: "Time Range" from the search bar got a little wonky
 - Regression Fix: Long messages in search results were not expanding upon click
 - Regression Fix: "Search using filters from this widget" went missing


Release Notes - Version 5.82
---

API

* Feature

 - Converted all syslog-ng rules and patterns to parser rules at `/etc/logzilla/rules.d`
 - Added `comments` field capability to parser rules
 - Added basic LDAP support
 - Added basic Office365 LDAP support

* Bug

 - ParserModule improvements
 - deb postinst was creating duplicate lines in `/etc/default/sec`
 - Parser restart on high EPS servers caused oot
 - Removed ip src/dst rule from distribution
 - Malware iocs were not auto-updating
 - Parser rule for junk programs renamed so that it fires later.
 - `lz5dashboards export -l` was not listing available dashboard ID's

 UI
 
 * Feature
 - Added "Apply" button when setting custom time ranges

* Bug
 - Red asterisk on settings>generic was missing description
 - UI Dashboard export broken on Firefox
 - Report generator was failing under some conditions.
 - Query parameter cache allowed an incorrect number of search results

Release Notes - Version 5.81
---

API

* Feature
 - Added API pull from AlienVault's [Open Threat Exchange](http://otx.alienvault.com) which will automatically download the latest IoC's (indicator of compromise) such as Malware/Blacklists, etc. and add them as an [parser rule](/help/data_transformation/rewrite_rules).

 
* Bug
 - Query Update Module would throw a seg fault during calculation of `LastN` widgets. This would cause "spinning widgets" with no data in some cases.
 - After backend model update, adding groups was borked. We unborked it.
 - GeoIP lookup's for IP's disappeared from the right-click menu on the search results page. We found him hiding in South America and made him come home ;)

 
UI

* Bug
 - Add widget display has misaligned descriptions


Release Notes - Version 5.80
---

API

* Feature
  - Replaced all default dashboards for new installs with the ones from LogZilla's [GitHub](https://github.com/logzilla/extras/tree/master/dashboards) account. Note: new dashboards will only be included during **new** installs, if upgrading, please visit [GitHub](https://github.com/logzilla/extras/tree/master/dashboards) for instructions.
  - Added many new enhancements to the [parser rewrite](/help/data_transformation/rewrite_rules) feature including RegEx captures, ability to drop messages, and dynamic key/value pair recognition from RFC5424 events.

UI

* Feature
  - Many UI usability enhancements including FontAwesome 5 glyphs.
  - Added ability to run a query based on the filters set in a widget.

* Bug
  - Ability to use boolean values in text search were borked, we unborked them.
  - Counters displayed `g` instead if `b` (for `billion`) when showing total events in the server.
  - Enter key was not performing a search after inputting search terms (users had to click the *search* button. 
  - GeoIP lookup map had a misleading *close* icon. 
  - Context-sensitive filter menu would sometimes appear off-screen when close to the search ribbon.
  - Querying invalid DNS lookups (for non-existent or internal IP's) would throw a 500 internal error instead of just telling the user it was an invalid IP.
  - Some UI icons were missing when using Chrome. We found them...hooray!



Release Notes - Version 5.79
---

* Feature
 - Enable rewrite rules to use grouped matches while rewriting

* Bug
 - apt-get dist-upgrade caused timeout when postgres was upgraded. LZ would restart automatically, but it was ugly. So we made it pretty.

Release Notes - Version 5.78
---

* Maintenance
 - Maintenance release - nothing noteworthy :) 


Release Notes - Version 5.77
---

API

* Story
 - As a large enterprise customer, I need to have triggers on the most actionable Cisco events

* Task
 - Improve future events buffer
 - Move Config outside the api.model
 - Allow Regex Patterns in `/etc/logzilla/rules.d` Rewrite Rules
 - Use storage filtering in queries
 - Internal counter cleanup
 - The version of syslog-ng installed should match the version in the syslog-ng.conf (fix for Balabit bug)
 - Unable to pass logs containing unicode into a trigger script
 - add support for INFLUXDB v1.3
 - Make sure tps is always sorted
 - Influx bug causes archive problems
 - Fix broken config migration for older versions
 - Remove absolute file path from logs

* Bug
 - lz5sender test tool is missing the option to use tcp instead of udp
 - Kaboom should not remove custom files in `/var/lib/logzilla/scripts`
 - Unable to import a single trigger (all triggers work)
 - Influx parse error

UI

* Story
 - UI: Add display warnings for disk full alert

* Task
 - Make phone field not required in the UI registration
 - Users should be asked to confirm when deleting a dashboard
 - Change "Search Cisco.com for this Mnemonic"


Release Notes - Version 5.76
---

* Feature
 - Add event filters to storage
 - Rewrite parser workers to use threads

* Bug
 - Fixed bug in multiple ParserWorkers 
 - Excluding > 1 host made a widget not filter anything

 

Release Notes - Version 5.75
---

* Feature
 - Added 900+ preconfigured Cisco Alerts
 - Allow multiple rewrite rules to be read from `/etc/logzilla/rules.d

* Task
 - Rewrite parser workers to use threads
 - Allow User Tags in rewrite rules
 - Move /etc/logzilla* files to its own dir under /etc/logzilla
 - Make lz5archive/restore work "offline"
 - lz5manage/setup should only warn if syslog-ng is not running

 * Bug
 - `.deb` postinst missing apache restart
 - Fixed intermittent problems with multiple ParserWorkers


Release Notes - Version 5.74
---

* Feature
 - Users may now share search result links

Release Notes - Version 5.73
---

* Task
 - API: Add a UI option to register evaluation license

* Bug
 - API: CPP filters - fix exclude operator (NE)
 - Fixed QueryUpdateModule WARNING queries_live_update_events
 - Modifying dashboards widgets should check dashboard owner

Release Notes - Version 5.72
---

* Feature
 - Ability to import and export Dashboards
 - Implemented multiple pre-built dashboards

* Task
 - Improvements on lz5query command

* Bug
 - Add widget modal had duplicated widget types in some browsers

Release Notes - Version 5.71
---
* Feature
 - Added tag rules for Windows-based events
 - Added autoarchive and retention options to the UI
 - Added pre-built triggers for Cisco and Windows

* Bug
 - Autoarchive was not updating storage counters post-archive
 - "Save To Dashboard" from search results was not saving to dashboard.
 - Modifying HH:MM:SS on search query bar was causing a search to start prior to actually clicking search.


Release Notes - Version 5.70
---
* Feature
 - Added ability to search data using prefix wildcards
 - Added ability to change the min word indexing length
 - Added ability to set custom time ranges for Seconds value
 - Added ability to configure LogZilla not to use any auth methods

* Task
 - API: Add simple cache for chunk counters
 - API: Add a cache for influx dictionaries

* Bug
 - set `LOG_INTERNAL_COUNTERS` default value to False
 - UI: Demo license is blank with only an exclamation
 - Creation of new users or triggers would not show until after a browser refresh
 -

Release Notes - Version 5.69
---
* Task
 - Query progress bar improvements
 - Better in-progress reporting for search queries
 - freeze_time option for queries
 - Remove time zone option from UI Settings page
 - Add EULA_ACCEPTED to settings

* Bug
 - Check for and remove rest_framework_swagger
 - Mnemonic right-click fails if it contains a %
 - Fix indexer crash bug
 - license EPD exceeded bug
 - StorageStats query return null results for today preset



Release Notes - Version 5.68
---

* Task
  - Create new trigger destination for Webhooks
  - Improve TopN performance
  - Added retention policy to rusage db

* Bug
  - Fix query processing for relative past time range
  - Allow users to format outgoing webhooks
  - Query update memory crash



Release Notes - Version 5.67
---

* Task
 - Added storage sync writes for performance improvement
 - Fix diskfree-alert in deb package

* Bug
 - Query initial values for some time zones were invalid
 - Fixed query updates on new events during initialization

Release Notes - Version 5.66
---

* Task
 - Remove duplicate trigger notifications
 - Timerange validator Improvements
 - Fix diskfree-alert in deb package



Release Notes - Version 5.65
---

* Bug
 - Filter corruption when new tag contains empty value


Release Notes - Version 5.64
---


* Task
 - Add ability to run 'or' boolean queries (Part 1 of 3)
 - Display Widget selected time ranges in widget title bar


Release Notes - Version 5.63
---


* Task
 - Added command line `lz5dashboards` command for import and export of custom dashboards. - Removed references to deprecated Graphite/Carbon/Whisper
 - Added Author and Author Email to Trigger environment variables
 - Disk IOPS widget now uses negative scale similar to Bandwidth Utilization
* Bug
 - Widget gauges do not show up until turned off and on again
 - Pie slices not clickable on some of the slices
 - Unable to expand message text when it is displayed in a widget
 - Network Widget should show Bps/Kbps/Mbps/Gbps and not be stacked
 - Creating a new user with the same name as a deleted one fails with no error
 - Add New Dashboard failing for some browsers
 - Dedup settings update causes spinner on some browsers
 - Dashboard time change not working in some browsers


Release Notes - Version 5.62
---

* Task
 - Create separated queues for tasks

* Bug
 - lz5manage and lz5setup should check for dependency connections and wait (with timeout)
 - Search results caching causes incorrect count of matches

