 <!-- @@@title:Correlating Windows Events@@@ -->


Receiving Windows Events
---
Sending Windows Events as syslog messages requires a third party program like Snare [Snare](/help/receiving_events_from_other_systems/receiving_windows_events). Once Snare (or a similar tool) is installed and your Windows system is sending events to LogZilla, no further configuration is necessary in order for LogZilla to be able to parse them.

Sample Windows Logs
--- 
Once Snare (or a similar tool) is sending events to LogZilla, messages will begin to appear in the LogZilla UI, for example:

*Sample 1:*

    Jul 28 20:50:09 10.1.1.50 78LYH8R MSWinEventLog 0 Security 406 Wed Jul 28 20:50:09 2004 592 Security JBrown User Success Audit 78LYH8R Detailed Tracking A new process has been created: New Process ID: 2004 Image File Name: \WINNT\system32\notepad.exe
   Creator Process ID: 1416 User Name: JBrown Domain: 78SYH8R Logon ID: (0x0,0x15923)   0

*Sample 2:*

    Jul 28 20:53:17 10.1.1.50 78LYH8R MSWinEventLog 0 Security 420 Wed Jul 28 20:53:17 2004 592 Security JBrown User Success Audit 78LYH8R Detailed Tracking A new process has been created: New Process ID: 1660 Image File Name: \WINNT\system32\CMD.EXE Creator Process ID: 1416 User Name: JBrown Domain: 78LYH8R Logon ID: (0x0,0x15923) 14

*Sample 3:*

    Jul 28 20:51:36 10.1.1.50 78LYH8R MSWinEventLog 0 Security 415 Wed Jul 28 20:51:36 2004 593 Security JBrown User Success Audit 78LYH8R Detailed Tracking A process has exited: Process ID: 2004 User Name: JBrown Domain: 78LYH8R Logon ID: (0x0,0x15923) 9

**Problem**

`Sample 1` is a notification that the notepad application was started (process ID 2004). `Sample 2` indicates that process ID 888 exited - however, there's no way to tell the name of the process that exited without tracking all processes as they start up, then recording the process name along with the process ID. When there are thousands of processes per minute in a busy system, matching the exit status of a process would be very resource intensive.

`Sample 3` is the start of the Windows `CMD.EXE` (the command line interface). The last entry is the shutdown of the notepad application noted in the first entry.

Given the examples above, we can track the Process Creation event type using the following rule to detect these events.


    #
    # Snare syslog Rule for Process Creation
    #
    
    type=Single
    ptype=RegExp
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+(\S+)\s+MSWinEventLog\t\d+\tSecurity\t\d+\t.*?\t(\d+)\tSecurity\t\S+\t\S+\tSuccess Audit\t(\S+)\tDetailed Tracking.*?Process ID: (\d+).*?Name:\s+(.*?)Creator Process ID: (\d+)\s+User Name: (\S+)\s+Domain: (\S+)\s+Logon ID: (\S+)\s+\d+.*
    desc=$0
    action=write - Found pattern [%s]

The pattern contains backreferences for Host or IPaddr, System Name, Windows Event ID, Hostname, new process ID, new process name, parent process ID, User Name, Windows domain, and Logon ID. However, if you prefer just the essentials, use:
  
    pattern=\S+\s+\d+\s+\S+\s+(\S+)\s+(\S+)\s+.*?New Process ID: (\d+).*?Name:\s+(.*?)Creator.*

Which contains backreferences for Host or IPaddr, System Name, new process ID, and new process name.
