<!-- @@@title:Receiving Windows Events@@@ -->


Tools
========

Several tools are available for converting and sending events from a Windows-based PC.

In the example below, we will use SNARE, <a href="http://sourceforge.net/projects/snare/files/">available here</a>.

During the setup process, select the default choices:

![Snare Auditing](@@path/images/snare-001.png)

![Service Account](@@path/images/snare-002.png)

For remote control, select the method which best suits your environment:

![Remote Admin](@@path/images/snare-003.png)

After install, select `Snare for Windows` from you program group, or browse to `http://localhost:6161/`

The default Login Username is `snare`, the password is the one created during the install when selecting "Enable Web Access" (see above)

Select `Network Configuration` from the left menu and set the following parameters:

* Destination Snare Server address
 - The IP or DNS name of your LogZilla server (make sure you can ping the server from this host)
* Destination Port
 - The standard port for UDP syslog is 514. Use this port unless you specified a different port in your LogZilla/syslog-ng config.
* Enable SYSLOG Header
 - Select the check box to enable
* Facility
 - Select the facility of your choice. We recommend `Local3`
* Priority
 - DYNAMIC

> Important: be sure to check the box `Enable SYSLOG Header?`

![Enable Syslog Header](@@path/images/snare-004.png)


Leave the other settings at their defaults and click `Apply the Latest Audit Configuration`

Windows Encoding
---
In some countries, the Windows Encoding may send non-UTF-8 characters.
This will cause the receiving syslog server (LogZilla in this case) to display "strange" characters.
We recommend configuring your server to send using UTF-8, but in the event you are unable to do so, you may need to modify
your LogZilla/syslog-ng configuration to match what your windows servers are sending.

    source src_win {
      udp(
            ip("x.x.x.x")
            port(514)
            encoding("WINDOWS-1252"));
    };

Where x.x.x.x is the IP (or regular expression) of your windows server(s).
