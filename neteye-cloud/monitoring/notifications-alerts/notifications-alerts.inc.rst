.. _nec-notifications-section:

Notifications and Alerts
~~~~~~~~~~~~~~~~~~~~~~~~

When monitoring finds a problem, it needs to let a certain set of people know
that something unusual or important has happened.  This requires knowing:

* **Exactly who to send a message to**, which could be a single person, a group
  of people, or even a different person depending on the characteristics (e.g.
  a Windows host or a Linux host) of the object having the problem, or the
  severity of the issue. In |nec| this is managed with *Contacts* and *Contact
  Groups*, and annotations on monitored objects.
* **What method to use to send that message**, which can depend on whether an
  operator is currently logged in or not, and how urgently the contact(s) need
  to respond. Message methods may include onscreen notes, email, or SMS messages.
* **The content that needs to be communicated**, including the monitoring
  objects affected, their current state, a time window, and potentially a
  recommended course of action.
* **How important the message is**, where more urgent messages may require
  more immediate media, such as telephone alerts instead of email.

In general, *notifications* are informational messages about changes that
users should be aware of, but are less important than *alerts*, which mean
that something serious is occurring and action needs to be taken quickly.

|nec| helps you define these parameters, even including setting up
`an SMS gateway <sms-gateway-moxa>`_ to get immediate alerts to your phone.

https://neteye.guide/4.48/core-modules/director/notifications.html


Contacts, Contact Groups and User Templates
```````````````````````````````````````````

In |ne|, a *contact* represents an end user who can receive
notifications. Each contact must be assigned at least one e-mail address,
which |ne| uses as the default delivery address for notifications.

Every contact is configured with two additional preferences: the time periods
during which that person is willing to receive notifications, and the
specific monitoring object states and transitions they are interested in.
This allows fine-grained control over who is notified, when, and under
what circumstances.

A *contact group* is a named collection of individual contacts. Rather than
assigning contacts one by one to each host or service object, an administrator
can assign a contact group to that object, and all members of that group will
be treated as eligible recipients for notifications pertaining to that object.

This simplifies configuration considerably in environments with a large number
of objects and users, because changes to group membership are automatically
reflected across all objects referencing that group.

.. _figure-nec-contacts-example:

.. figure:: /neteye-cloud/monitoring/img/contacts-example.png
   :alt: Example contact details

   The Contacts interface and Contact Details panel

A *user* for notification purposes is

Just as we use Host Templates to make it easier to define large numbers
of hosts, we can define many users by using *User Templates*.  User
Templates are quite easy to create.

When creating a new user, the entry *must* inherit a user template, or else
it cannot be created. Some extra properties beyond what is in the template
must be filled in:

* A username (which is not the same as a |ne| login)
* A display Name (can include spaces and full punctuation)
* An email address

States and Transition types are inherited in a user template, and you can
only edit or replace inherited values. You can also add users to *User Groups*,
although user groups only support imperative assignment, you cannot assign
users to user groups by using a variable and an apply rule.

.. _figure-nec-user-for-notification:

.. figure:: /neteye-cloud/monitoring/img/user-for-notification.png
   :alt: Editing a User within a User Group

   Editing a User within a User Group

Understanding When and Why You Receive Notifications
````````````````````````````````````````````````````

When an object's status changes, |ne| determines whether a notification
should be sent for the new state. If so, it identifies all end users eligible
to receive notifications for that object, retains only those interested in
the specific notification type, and then delivers the message to each one.

Before a user can receive notifications, they must be added to the |ne|
*Contacts List* and provide an e-mail address that |ne| can use to reach them. Each
contact can independently configure two aspects of their Notification
preferences: the time windows during which they wish to receive Notifications,
and the Object states and transitions they are interested in.

Escalation of Notifications
===========================

*Escalation* is the process of notifying an end user of a change in the status
of a monitored host or service when it happens. |ne| collects information about
the issue and if certain conditions are met, that information is then handed
over to the system for communicating that to the end user.

So there are two elements: the information itself to be communicated, and the
communication method, which is the infrastructure that can deliver that
information, such as an e-mail server or an SMS gateway.

How |ne| produces notifications
===============================

For Host Objects, the available states are Up, Down, and Unreachable. For
Service Objects, the available states are OK, Warning, Critical, and Unknown.
Transitions, applicable to both Host and Service Objects, are grouped into
three categories:

* State changes: Problem, Recovery, and Custom
* Problem handling: Acknowledgement, and Downtime start, end, and removed
* Flapping: Flapping start and end

As a result, a user will only receive notifications for specific states
during specific time windows, and no notifications that fall outside those
parameters.

For a monitoring object to send notifications, it must be configured with the
following:

* A list of Users to contact
* A list of states for which notifications should be sent
* The time windows during which notifications should be sent
* Parameters governing delayed and repeated sending

A user will thus receive a notification from an object only when all of
the following conditions are met simultaneously: The user is included in the
object's Contact List, the object is configured to send notifications for the
current state the object is in, and the user is marked as willing to receive
notifications about that state.

In addition, no notifications will be sent if the object is in scheduled
downtime, in an Unreachable state, or if its current state has been
acknowledged.

.. _figure-nec-notification-example:

.. figure:: /neteye-cloud/monitoring/img/notification-example.png
   :alt: Interface for creating notifications

   Creating a User Template for notifications: *notify-AllEvents*

Controlling Which States Trigger a Notification
```````````````````````````````````````````````

To view or edit the states that trigger notifications, you should use one
of the built-in user templates provided by |ne| that can be used as a base
for new contacts. Any user inheriting from these templates will be configured
to receive all types of notifications by default.

It is important to understand that it is not enough to mark a user as being
interested in a given state: users must also be marked for a set of Problems
or Recovery Transitions.

.. _figure-nec-escalation-user-templates:

.. figure:: /neteye-cloud/monitoring/img/escalation-user-templates.png
   :alt: Editing notification states and transitions

   Editing notification states and transitions

Notification Templates and Apply Rules
======================================

Notification templates define:

* When a notification should be produced
* For which states
* Delivered to whom and how

A notification is first sent after *First notification delay* seconds, and
is then resent after every *Notification interval* seconds.  The *Last
notification* defines how many seconds until no more notifications are sent
for this event.  All of these apply only while the same state is still active.

When selecting the States, be sure to not mix states that apply to host
objects with those states that apply to service objects.

.. _figure-nec-notification-apply-rules:

.. figure:: /neteye-cloud/monitoring/img/notification-apply-rules.png
   :alt: Notification templates for hosts versus services

   Notification templates for host objects versus service objects

Because of |ne| 4 Community Repository, there are already some notification
templates available, with self-explanatory names.  These templates are
separated so that some are intended for host objects and some for
service objects.  This separation should be maintained in order to avoid
future problems when deploying a new configuration.

.. _figure-nec-nep-notification-templates:

.. figure:: /neteye-cloud/monitoring/img/nep-notification-templates.png
   :alt: Community-provided notification templates
   :width: 50%

   Community-provided notification templates

A notification template is applied to a monitoring object by using a
*Notification Apply Rule*, which can rewrite all the properties of a
template. The following elements are included:

* An *Assign where* clause that tells which monitoring objects the
  notification is applied to
* An *Apply to* property, which sets the target type of the *Assign where*
  clause: host object or service object

This is where the virtual separation mentioned above becomes important:

* If you apply a notification to hosts and it involves a service state (OK,
  Warning, Critical or Unknown), you will get a Deploy error
* If you apply a notification to services and it involves a host state
  (Up or Down), you will again get a Deploy error

.. _figure-nec-notification-apply-conflicts:

.. figure:: /neteye-cloud/monitoring/img/notification-apply-conflicts.png
   :alt: Assigning notification templates via apply rules

   Assigning notification templates via *apply rules*

Notification via Email
======================

|ne| will never directly interface with your mail server(s). Instead it has
its own Postfix-based internal email server, used for both sending and
receiving emails.

You'll have to configure it for interfacing with your own servers, where
the easiest configuration method is using an anonymous relay:

* Insert the following line into the file */etc/postfix/main.cf*:

  * relayhost = [your mail server name]

* Restart the postfix service:

  * systemctl restart postfix.service
