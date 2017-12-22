<!-- @@@title:Debugging Event Reception@@@ -->

>This document is provided only as a helpful guide. LogZilla Corporation does not provide support for products outside of our own software.

# No Events In LogZilla
If you are not receiving events from other systems, there are several ways to determine the cause

## Use `tcdump`
The first step is to use `tcpdump` on the LogZilla server to determine if your remote host's events are even reaching the LogZilla server:

    tcpdump -vv -i eth0 udp port 514
this will listen on your "eth0" adapter for incoming events on port 514 (the default for UDP syslogs). If you have a different Ethernet interface name or are sending to a different port, please change the above command accordingly, for example:

    tcpdump -vv -i p1p1 tcp port 601

If you receive events on udp port 514, tcpdump will decode the log messages automatically:

    tcpdump -vv -i eth0 udp port 514

    tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
    17:01:01.955523 IP (tos 0x0, ttl 64, id 44193, offset 0, flags [DF], proto UDP (17), length 272)
    25.92.104.22.57053 > logzilla.myserver.com.syslog: [udp sum ok] SYSLOG, length: 244
        Facility kernel (0), Severity warning (4)
        Msg: Sep  3 13:01:02 www kernel: [UFW BLOCK] IN=eth0 OUT= MAC=01:22:33:02:e5:01:44:c5:9c:f9:18:30:08:00 SRC=191.168.1.2 DST=10.2.1.6 LEN=60 TOS=0x00 PREC=0x00 TTL=44 ID=65267 DF PROTO=TCP SPT=41410 DPT=22 WINDOW=14600 RES=0x00 SYN URGP=0 \0x0a
        0x0000:  3c34 3e53 6570 2

# Start Syslog-ng in Debugging Mode

If you find that events are being received from tcpdump but still not appearing in LogZilla, the next step is to verify that syslog-ng is processing them properly. Start syslog-ng in debug mode:

    service syslog-ng stop
    syslog-ng -Fdve

This will list the modules, filter rules and nodes, log files, sources, and destinations that are loaded at start up. Open a separate console window, and run the following command:

    logger test

You'll see the command's success or failure in the debugging display. Here's an example of the output:

    Incoming log entry; line='<86>Oct  1 14:24:18 su[8423]: pam_unix(su:session): session opened for user root by ubuntu(uid=0)

Any errors displayed should help narrow down any communication issues you are having.

Once you are finished troubleshooting, remember to stop the `syslog-ng -Fdve` debug and re-start it the normal way using `service syslog-ng start`.

If you are still unable to receive events, you can contact us by visiting http://support.logzilla.net and attaching the output from the following command:

    # Options below are (i)nterface (p)ort (s)econds
    # 10 Minutes = 1200 seconds
    # 1 day = 86400
    wget -qO- http://repo.logzilla.net/tools/tcpcap | sudo bash -s -- -i eth0 -p 514 -s 1200

Be sure to include your installed LogZilla version which can be found at the bottom right corner of the Web Interface once logged in.
