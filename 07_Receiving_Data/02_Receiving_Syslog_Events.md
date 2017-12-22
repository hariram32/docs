<!-- @@@title:Receiving Syslog Events@@@ -->


LogZilla's Syslog-ng Configuration
---

LogZilla includes configuration files for `syslog-ng` to automatically start receiving events as soon as LogZilla is installed.

Each file name is separated by a `.` indicating its role, such as a `global` configuration or `sources` (which sets up the network listener).

For example:

```
001.logzilla.global.conf
002.logzilla.sources.conf
004.logzilla.templates.conf
005.logzilla.destinations.conf
006.logzilla.log-outputs.conf
```

Prior to LogZilla `v5.81`, there were also several rewrite rules included in this directory. As of LogZilla `v5.81`, however, rewriting incoming events is now much easier and faster using LogZilla's built-in [Rewrite feature](/help/data_transformation/rewrite_rules)


# Forwarding through other systems
For help with configuring remote relays to send events into LogZilla, see [Relays](/help/receiving_data/relays)

