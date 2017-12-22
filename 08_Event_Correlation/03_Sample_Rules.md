<!-- @@@title:Sample Rules@@@ -->


LogZilla comes pre-installed with a set of sample rules to help users get started.

These rules are located on this server at `/etc/sec/logzilla/` and contain many useful rules for Correlating events.

Correlating Cisco Events
---
* Modify `/etc/sec/logzilla/lz-cisco.sec` and change the destination email address
 - The default is set to root@localhost
* Feel free to comment out/modify any rules you like.
* Set up a trigger in the LogZilla `UI>Triggers` menu to match on the program name, "Cisco".

![Trigger Event Correlation Rule](@@path/images/cisco-ec.png)

Note here that rather than sending *all* Cisco events to the event correlation rule, you may also further refine your matches to any other items in this list such as individual Cisco Mnemonics, or only Cisco events with the word "failed", etc.

![Refine Trigger Matching](@@path/images/cisco-ec-mne.png)

* Configure this trigger to "Mark as Actionable" and "Execute a Script", then choose the included "cisco-event-correlation" script as the destination and "Save changes"

![Select Options for EC Rule and Save](@@path/images/cisco-ec-save.png)


Scripts
---
The Script used to call the event correlation (located at `/var/lib/logzilla/scripts/cisco-event-correlation`) is a very simple script which forwards the event to the Correlation engine. For example:

    #!/bin/bash
    # Forwards events to a log file which is read by SEC for processing rules
    printf "%s %s\n" "$EVENT_HOST" "$EVENT_MESSAGE" >> /var/log/logzilla/triggers.log

As you create your own triggers for various correlation rules, it is important to keep in mind the impact of scalability when processing large amounts of events (>1000 per second). Depending on your server's capability, this may degrade performance. Thus, using a log file for SEC to follow helps to ensure that no events are missed, whereas using something like a Named Pipe may lose events due to lack of system resources.

Note also that we only need the sending Host and the actual Message. Other columns such as time stamp, facility, severity, etc. are not necessary in this context. This makes your Regular Expression patterns faster when matching on 1000's of events.

Sample Rules File
---
A `.sec` file is a set of rules which tell the correlator how to match and process incoming events, as noted in the <a href="/help/event_correlation/event_correlation_rule_types">Rule Types</a> section of this guide.

Looking at the first three rules in `/etc/sec/logzilla/lz-cisco.sec`, we can see:

    # ----- Process reload and restart events -----

    # Looks for a reload
    #
    type=single
    continue=takeNext
    ptype=regexp
    pattern=(\S+) .?SYS-5-RELOAD: (.*)
    desc=(WARNING) reload requested for $1
    action=pipe '%s details:$2' mail -s '[LogZilla] Cisco Alert on $1' root@localhost

    # Looks for a reload followed by a restart event
    #
    type=pairWithWindow
    ptype=regexp
    pattern=(\S+) .?SYS-5-RELOAD:
    desc=(CRITICAL) $1 RELOAD_PROBLEM
    action=pipe '%s' mail -s '[LogZilla] Cisco Alert on $1' root@localhost
    ptype2=regexp
    pattern2=($1) .?%SYS-5-RESTART:
    desc2=(NOTICE) $1 RELOAD_OK
    action2=pipe '%s' mail -s '[LogZilla] Cisco Alert on $1' root@localhost
    window=300

    # Looks for a restart without reload command
    #
    type=single
    ptype=regexp
    pattern=(\S+) .?%SYS-5-RESTART:
    desc=(CRITICAL) $1 restart without reload command
    action=pipe '%s' mail -s '[LogZilla] Cisco Alert on $1' root@localhost

These three rules all share the same "flow", meaning that they work together to form a full Correlation.

Referencing the <a href="/help/event_correlation/event_correlation_rule_types">Rule Types</a> help page, we see that the rules used here are `Single` and `pairWithWindow`.

The first rule waits for a reload event sent by LogZilla. The Regex pattern used here is easy because we only need to send the Host and Message from LogZilla in order to get the rule to trigger.

The next rule, pairWithWindow, tells the event correlator to wait for 5 minutes (300 seconds), to receive a `RELOAD` event followed by a `RESTART` event. If it does not arrive within 5 minutes, send an email.

The last rule tells the EC to check for a `RESTART` event in case no prior `RELOAD` event has been seen.
