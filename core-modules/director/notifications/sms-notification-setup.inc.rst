SMS Notification Setup
~~~~~~~~~~~~~~~~~~~~~~

After you have properly setup a SMS modem you should configure
notification. To achieve this, you need to create suitable *users* and
*notifications*. The whole process requires to create new *data field*
and *commands*, then *notification templates* and *user templates* to
define *users*, and finally to use them within a *rule*.

.. note:: In case of a cluster installation you need an SMS modem connected to
   **each** node. It is **not** possible to use a single Moxa
   configured on two different nodes.

Data Field Creation
```````````````````

Create a data field with following parameters:

-  Field name = "user\_sms"
-  Caption = "User SMS"

It will be necessary when defining new user templates, to allow them to
access the SMS functionality.

Commands Creation
`````````````````

Now you have to create two commands, one for the hosts and one for the
services, necessary to activate SMS notification for hosts and services
respectively.

The **command for the hosts** needs the following parameters:

* Command type = "Notification Plugin Command"
* Command name = "sms-host-notification"
* Command = "/usr/local/bin/sms-host-notification.sh"
* Timeout = 60
* Disabled = "no"

Arguments:

+------------+--------------------------------------+
| Argument   | Value                                |
+============+======================================+
| -4         | $address$                            |
+------------+--------------------------------------+
| -6         | $address6$                           |
+------------+--------------------------------------+
| -b         | $notification.author$                |
+------------+--------------------------------------+
| -c         | $notification.comment$               |
+------------+--------------------------------------+
| -d         | $icinga.long_date_time$              |
+------------+--------------------------------------+
| -f         | $notification_from$                  |
+------------+--------------------------------------+
| -i         | $notification_icingaweb2url$         |
+------------+--------------------------------------+
| -l         | $host.name$                          |
+------------+--------------------------------------+
| -n         | $host.display_name$                  |
+------------+--------------------------------------+
| -o         | $host.output$                        |
+------------+--------------------------------------+
| -r         | $user_sms$                           |
+------------+--------------------------------------+
| -s         | $host.state$                         |
+------------+--------------------------------------+
| -t         | $notification.type$                  |
+------------+--------------------------------------+
| -v         | $notification_logtosyslog$           |
+------------+--------------------------------------+

Fields:

* Field = "user\_sms"
* Mandatory = "Mandatory"

Create a **command for the services** with the following parameters:

* Command type = "Notification Plugin Command"
* Command name = "sms-service-notification"
* Command = "/usr/local/bin/sms-service-notification.sh"
* Timeout = 60
* Disabled = "no"

Arguments:

+------------+--------------------------------------+
| Argument   | Value                                |
+============+======================================+
| -4         | $address$                            |
+------------+--------------------------------------+
| -6         | $address6$                           |
+------------+--------------------------------------+
| -b         | $notification.author$                |
+------------+--------------------------------------+
| -c         | $notification.comment$               |
+------------+--------------------------------------+
| -d         | $icinga.long_date_time$              |
+------------+--------------------------------------+
| -e         | $service.name$                       |
+------------+--------------------------------------+
| -f         | $notification_from$                  |
+------------+--------------------------------------+
| -i         | $notification_icingaweb2url$         |
+------------+--------------------------------------+
| -l         | $host.name$                          |
+------------+--------------------------------------+
| -n         | $host.display_name$                  |
+------------+--------------------------------------+
| -o         | $service.output$                     |
+------------+--------------------------------------+
| -r         | $user_sms$                           |
+------------+--------------------------------------+
| -s         | $service.state$                      |
+------------+--------------------------------------+
| -t         | $notification.type$                  |
+------------+--------------------------------------+
| -u         | $service.display_name$               |
+------------+--------------------------------------+
| -v         | $notification_logtosyslog$           |
+------------+--------------------------------------+

Fields:

* Field = "user\_sms"
* Mandatory = "Mandatory"

Notification Template Creation
``````````````````````````````

Now, by using the commands defined in the previous section, set up two
new notification templates as follows:

*Host notification template*:

* Notification Template = "sms\_host\_notification"
* Notification command = "sms-host-notification"
* States = Down, Up
* Transition types = Acknowledgement, Custom, DowntimeEnd,
  DowntimeRemoved, DowntimeStart, FlappingEnd, FlappingStart, Problem,
  Recovery

*Service notification template*:

* Notification Template = "sms\_service\_notification"
* Notification command = "sms-service-notification"
* States = Critical, OK, Unknown, Warning
* Transition types = Acknowledgement, Custom, DowntimeEnd,
  DowntimeRemoved, DowntimeStart, FlappingEnd, FlappingStart, Problem,
  Recovery

Notifications - Users - User Templates Creation
```````````````````````````````````````````````

We can now create a user template with following parameters:

* User template name = "notify\_allEvents"
* Send notifications = "yes"
* Custom properties = "User SMS"

Fields:

* Field = "user\_sms"
* Mandatory = "Optional"

User configuration Creation
```````````````````````````

To create a user that is allowed to send SMS, import the template
*notify\_allEvents* and specify *User SMS* as Custom property.

Notification Apply Rule Creation

As last step, create an *apply rule* with following parameters:

-  Imports = "sms\_host\_notification"
-  Users = "*your\_user*\ ", i.e., the user created in the previous
   section.
-  Apply to = "Hosts"
-  Assign where "host.name" = "\*"
