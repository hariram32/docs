<!-- @@@title:Relays@@@ -->

As noted in [Syslog Basics](/help/receiving_data/syslog_basics), relays are used to forward events from other sources to another server that needs to receive those logs (like LogZilla).


# Syslog-ng

If your relay host uses syslog-ng, the following file may be used to forward events to LogZilla.

```
# This is for your *relay* server (not the LogZilla server)
# filename: /etc/syslog-ng/conf.d/logzilla-relay.conf
 
#Global Options
options {
  flush_lines(100);
  threaded(yes);
  use_dns(yes);
  use_fqdn (no);
  keep_hostname (yes);
  dns-cache-size(2000);
  dns-cache-expire(87600);
};
 
source s_network {
 
# port 514 (tcp) is used for RFC3164 formatted events coming in (standard BSD-style logs)
  network(
      transport("tcp")
      port(514)
  );
 
# port 514 (udp) is used for RFC3164 formatted events coming in (standard BSD-style logs)
  network(
      transport("udp")
      so_rcvbuf(1048576)
      flags("no-multi-line")
      port(514)
  );
 
# port 601 is for RFC5424 formatted events coming in (key=value pairs)
  network(
      transport("tcp")
      flags(syslog-protocol)
      port(601)
  );
};
 
 
destination d_logzilla {
 tcp("<IP OR HOSTNAME OF LZ SERVER>" port(514));
  # Or, if forwarding RFC5424-style events, use:
  # tcp("<IP OR HOSTNAME OF LZ SERVER>" port(601));
  );
};
 
log {
    source(s_logzilla);
    # disable s_src if you don't want local server events
    source(s_src);
    source(s_network);
    destination(d_logzilla);
    flags(flow-control);
};
```


# Rsyslog

> Rsyslog configuration is provided only as a helpful guide. LogZilla Corporation does not provide support for products outside of our own software.

As noted in [Syslog Basics](/help/receiving_data/syslog_basics), there are two formats used for the syslog protocol. Users may configure either RFC 3164 based forwarding or RFC 5424 based forwarding from their rsyslog relays.

### RFC 3164 (default)
To forward logs to LogZilla using the standard format, create a file in `/etc/rsyslog.d/` using a `.conf` extension and place the following lines in that file:

```
*.*   @@${logzillaIP}:514
```
Replace `${logzillaIP}` with the IP Address of your LogZilla server.

**Example using the `echo` command:**


* Replace the IP address assigned to the `logzillaIP` below, then copy/paste all lines.

(everything below must be done as root, use `sudo su -` to become root):

```shell
logzillaIP="172.16.1.100"
config_file="/etc/rsyslog.d/logzilla.conf"
echo "*.*   @@${logzillaIP}:514" > $config_file
service rsyslog restart
```

### RFC 5424

To send messages using the RFC 5242 method, replace `*.*   @@${logzillaIP}:514` above with:

```
*.*(o)@@${logzillaIP}:601;RSYSLOG_SyslogProtocol23Format
```

After restarting rsyslog, if messages come into LogZilla such as `invalid frame header` you may need to remove the `(o)` from your rsyslog sending server's config entry

For example:

```
*.*@@${logzillaIP}:601;RSYSLOG_SyslogProtocol23Format
```

