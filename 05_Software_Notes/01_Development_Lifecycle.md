<!-- @@@title:Software Releases@@@ -->


## Release Timing
New versions of LogZilla are released to the public `every two weeks`.
Our development team follows a process known as [Scrum](https://en.wikipedia.org/wiki/Scrum_%28software_development%29) so that we may bring new features and fixes to you at a much faster pace.  
You will notice that our version numbers also coincide with the sprint # that we are on. Thus, if you are using `v5.42` and we are on `v5.44`, then it means you are 2 revisions (4 weeks) behind on code improvements. 

## Development Lifecycle
Our development cycle for a ticket lasts `6 weeks` from start to end. This is because there are 3 stages that an enhancement or bugfix must pass before being released:

 - Development Branch
 - Staging Branch
 - Master Branch


![Ticket Lifecycle](@@path/images/ticketflow.png)


### Development Branch
Developers work locally on their own workstations to write and test the code on their own systems. Once they feel it is ready, they will push their changes to a separate branch in the code repository which is associated with the ticket number they are working on - at which time, the ticket is marked for `Peer Review`. 
Once the ticket is reviewed by one of their peers and passes, the code is then merged into the `Development` branch of our repository and marked as `QA ready`. The QA team checks the work on the development branch (which is automatically installed on a test server) to make sure everything looks good. 

At the end of each sprint, the associated work done on each ticket is demonstrated to the entire company by the developer who wrote the code. For example, if one of our UI developers writes a new feature to "send email", then he or she would then demonstrate that function during a company meeting held at the end of the sprint.

### Staging Branch
Once the code has passed QA and been demonstrated to the company stakeholders, it is then pushed to our staging branch (and deployed to a staging test server). At this time, the QA team checks the software for regression bugs. Meaning, they test LogZilla to find out if the introduction of the new code has broken any of the old code.

### Master Branch
After passing regression testing, the `Staging` branch is then merged to the `Master` branch and uploaded to our repository server for public accessibility. 
It is at this time that users will see the work done which started 6 weeks prior.

>Some users prefer to use staging or even development versions of LogZilla so that they get the latest updates even faster. Generally speaking, this is fine (we have only had 1 regression bug since we started). If you wish to run one of these branches, please contact your sales representative for simple instructions.

## Updating Your Software
It is important that users update their systems in order to make sure that all known bugs are addressed prior to opening a support ticket. Updates are very easy and use the exact same process to update as the installation. 
For example, if you installed LogZilla using:
`wget -qO- http://logzilla.sh | sudo bash -s verbose`
Then to update to the latest version, you would paste that very same command again.
LogZilla will detect whether it is a new installation or an update.

>After an update, be sure to check `/var/log/logzilla/logzilla.log` for any errors. If you do see a problem and need assistance, we are happy to help.

