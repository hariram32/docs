<!-- @@@title:Intro to Event Correlation@@@ -->


Event Correlation Methods
---
Event correlation generally includes the following concepts:
* Event triggers (when to correlate)
* Event filters (what to correlate)
* Event pairing (associations between multiple events)
* Event suppression (what to ignore)
* Time-based (window of time before something becomes important, or no longer important)

Event Correlation in LogZilla
---
LogZilla's triggers can be used to send matched events to a well-known tool called SEC (Simple Event Correlator). SEC is already installed with LogZilla along with some sample rules to help you get started.

Flow
---
SEC is traditionally used as the pre-processor for systems like this where a log message would be sent to SEC before coming in to LogZilla. However, because LogZilla scales to 100k EPS on a single server (and millions of EPS on multiple servers), SEC is not able to process such a large number of events.

Instead, we allow users to create triggers from within LogZilla and send only the events they want correlated (based on a matched triggers) to the SEC tool. Sending only the events you actually care about greatly reduces the amount of strain put on the Event Correlation process.

This method also has the added bonus of being able to correlate events from more than just syslog messages (e.g.: SNMP Traps, etc.).

Traditional Method
---
`Syslog Daemon --> SEC --> LogZilla`
Scalable Method
---
`<Syslog Daemon, SNMP Traps, any unstructured data, etc. --> LogZilla --> SEC`


About SEC
---
SEC was written by Risto Vaarandi and is available from the <a href="https://github.com/simple-evcorr/sec">SEC Github Page</a> as well as Debian-based Repositories (via apt-get)
