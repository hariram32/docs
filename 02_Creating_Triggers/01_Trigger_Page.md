<!-- @@@title:The Trigger Page@@@-->

Trigger Firing Order
---
Note that the order in which triggers are listed are the same order they will be matched upon (from top to bottom of the page). Once a match is made, no other triggers are processed. Thus, it is important that you start with the most finite matches and prioritize wider ranging matches further down the list. 

For example, a match on `interface` would match `interface GigabitEthernet1/0/1`, `interface GigabitEthernet1/0/2`, etc., then stop processing further rules. 

Instead, you may want a more finite match such as `GigabitEthernet1/0/1` to be ordered higher (or lower depending on the intent).

Creating a Trigger
---

In the LogZilla UI, click the 'Triggers' link in the top menu. There, you'll see a button near the top of the page 'Add new notification trigger', and below that a list of any triggers already created on your server. Clicking the button will allow you to create a trigger with no pre-set information selected. This is the easiest way to create triggers that will apply to the widest range of conditions.

If you'd like to monitor failed logins for all of your servers, this is the best place to do it. Simply click the button, give your new trigger a name, and enter your search criteria, 'failed login' in the 'Event match' section. By default, 'Issue Notification' is already selected, so for a system wide rule, that's all you need to do. Just click 'Save changes' and your trigger will be active.

![Add new trigger](@@path/images/add-new-trigger.png)
