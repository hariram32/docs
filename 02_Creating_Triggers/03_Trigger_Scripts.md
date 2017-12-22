<!-- @@@title:Trigger Scripts@@@-->


Trigger Throttling
---

These settings protect slower servers from being overtaxed and from users who may accidentally write a trigger that matches on everything coming in. For example, if a user sets a match on a common word like `after` and your server is ingesting 50k EPS, then the trigger throttle will kick in to protect the server.

However, this will also affect all triggers, including marking events as Actionable/Non-Actionable. Thus, if a trigger is set to mark A/N and it is throttled, that marking will not occur.

Should throttling occur, users will see a warning entry in their `/var/log/logzilla/logzilla.log` indicating this.

The UI can also be set to notify on these problems as well by setting the appropriate warning level for internal events:

Enable UI notifications using the command line by typing:

    ~logzilla/src/bin/lz5configmanager -ss INTERNAL_EVENTS_MAX_LEVEL WARNING
    service logzilla restart

Use Event Correlation
---
If you wish to "throttle" email alerts, we recommend using <a href="/help/event_correlation/intro_to_event_correlation">Event Correlation</a> instead of the <a href="/help/creating_triggers/explanation_of_actions">Send E-mail</a> option. Event Correlation provides a lot more power for determining when to send an email alert.

## Script Types
LogZilla can take any type of executable script, for example:
- Perl
- Python
- sh, bash, zsh, csh, etc.
- Compiled Executables


## Script Environment
All triggers passed to a script contain all of the matched message information as environment variables.
To manipulate any of the data, simply call that environment variable.

The following list of variables are passed into each script automatically:

    EVENT_COUNTER=<integer>
    EVENT_CISCO_MNEMONIC=<string>
    EVENT_STATUS=<integer>
    EVENT_ID=<integer>
    EVENT_SEVERITY=<integer>
    EVENT_FACILITY=<integer>
    EVENT_HOST=<string>
    EVENT_FIRST_OCCURRENCE=<float>
    EVENT_SNARE_ID=<integer>
    EVENT_PROGRAM=<string>
    EVENT_LAST_OCCURRENCE=<float>
    EVENT_MESSAGE=<string>
    EVENT_TRIGGER_ID=<integer>
    EVENT_TRIGGER_AUTHOR
    EVENT_TRIGGER_EMAIL


# Calling a script in LogZilla
>Note: the path where your scripts are stored must match the value set in LogZilla's `Settings>System Settings>Triggers` menu option.

From an SSH Console/Shell:

1. Create a new file at /var/lib/logzilla/scripts/myscript
2. Add the script contents and save the file
3. Run the following commands to change ownership and permissions on the script:


    chown logzilla:logzilla /var/lib/logzilla/scripts/myscript
    chmod 755 /var/lib/logzilla/scripts/myscript

Next, log into the LogZilla Web Interface and:

1. Create a new trigger from the trigger menu
2. Select the `execute script` option.
3. Fill in the path to your new script, such as: `/var/lib/logzilla/scripts/myscript`

Any patterns matching this trigger will now be executed.

You may also find some useful scripts <a href="https://github.com/logzilla/triggers">on our GitHub page</a> to help you get started.
