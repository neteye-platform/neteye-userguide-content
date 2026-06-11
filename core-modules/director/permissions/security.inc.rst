.. _monitoring-module-security:

The monitoring module provides an additional set of restrictions and
permissions that can be used for access control. The following sections
will list those restrictions and permissions in detail:

The monitoring module allows to send commands to an Icinga 2 instance. A
user needs specific permissions to be able to send those commands when
using the monitoring module.

+-----------------------------------+----------------------------------+
| Name                              | Permits                          |
+===================================+==================================+
| monitoring/command/*              | Allow all commands.              |
+-----------------------------------+----------------------------------+
| monitoring/command/schedule-check | Allow scheduling host and        |
|                                   | service checks.                  |
+-----------------------------------+----------------------------------+
| monitoring/command/schedule-check | Allow scheduling host and        |
| /active-only                      | service checks. (Only on objects |
|                                   | with active checks enabled)      |
+-----------------------------------+----------------------------------+
| monitoring/command/acknowledge-pr | Allow acknowledging host and     |
| oblem                             | service problems.                |
+-----------------------------------+----------------------------------+
| monitoring/command/remove-acknowl | Allow removing problem           |
| edgement                          | acknowledgements.                |
+-----------------------------------+----------------------------------+
| monitoring/command/comment/*      | Allow adding and deleting host   |
|                                   | and service comments.            |
+-----------------------------------+----------------------------------+
| monitoring/command/comment/add    | Allow commenting on hosts and    |
|                                   | services.                        |
+-----------------------------------+----------------------------------+
| monitoring/command/comment/delete | Allow deleting host and service  |
|                                   | comments.                        |
+-----------------------------------+----------------------------------+
| monitoring/command/downtime/*     | Allow scheduling and deleting    |
|                                   | host and service downtimes.      |
+-----------------------------------+----------------------------------+
| monitoring/command/downtime/sched | Allow scheduling host and        |
| ule                               | service downtimes.               |
+-----------------------------------+----------------------------------+
| monitoring/command/downtime/delet | Allow deleting host and service  |
| e                                 | downtimes.                       |
+-----------------------------------+----------------------------------+
| monitoring/command/process-check- | Allow processing host and        |
| result                            | service check results.           |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/instan | Allow processing commands for    |
| ce                                | toggling features on an          |
|                                   | instance-wide basis.             |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/object | Allow processing commands for    |
| /*                                | toggling features on host and    |
|                                   | service objects.                 |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/object | Allow processing commands for    |
| /active-checks                    | toggling active checks on host   |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/object | Allow processing commands for    |
| /passive-checks                   | toggling passive checks on host  |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/object | Allow processing commands for    |
| /notifications                    | toggling notifications on host   |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/object | Allow processing commands for    |
| /event-handler                    | toggling event handlers on host  |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| monitoring/command/feature/object | Allow processing commands for    |
| /flap-detection                   | toggling flap detection on host  |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| monitoring/command/send-custom-no | Allow sending custom             |
| tification                        | notifications for hosts and      |
|                                   | services.                        |
+-----------------------------------+----------------------------------+

.. _monitoring-module-restrictions:

Restrictions
````````````

The monitoring module allows filtering objects:

+---------------------------+---------------------------------------------+
| Keys                      | Restricts                                   |
+===========================+=============================================+
| monitoring/filter/objects | Applies a filter to all hosts and services. |
+---------------------------+---------------------------------------------+

This filter will affect all hosts and services. Furthermore, it will
also affect all related objects, like notifications, downtimes and
events. If a service is hidden, all notifications, downtimes on that
service will be hidden too.

.. rubric:: Filter Column Names


The following filter column names are available in filter expressions:

+---------------------------------------+------------------------------+
| Column                                | Description                  |
+=======================================+==============================+
| instance_name                         | Filter on an Icinga 2        |
|                                       | instance.                    |
+---------------------------------------+------------------------------+
| host_name                             | Filter on host object names. |
+---------------------------------------+------------------------------+
| hostgroup_name                        | Filter on hostgroup object   |
|                                       | names.                       |
+---------------------------------------+------------------------------+
| service_description                   | Filter on service object     |
|                                       | names.                       |
+---------------------------------------+------------------------------+
| servicegroup_name                     | Filter on servicegroup       |
|                                       | object names.                |
+---------------------------------------+------------------------------+
| all custom variables prefixed with    | Filter on specified custom   |
| ``_host_`` or ``_service_``           | variables.                   |
+---------------------------------------+------------------------------+
