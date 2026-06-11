.. _monitoring-module-security:

The Icinga DB module provides an additional set of restrictions and
permissions that can be used for access control. The following sections
will list those restrictions and permissions in detail:

The Icinga DB module allows you to send commands to an Icinga 2 instance. A
user needs specific permissions to be able to send those commands when
using the Icinga DB module.

+-----------------------------------+----------------------------------+
| Name                              | Permits                          |
+===================================+==================================+
| icingadb/command/*                | Allow all commands.              |
+-----------------------------------+----------------------------------+
| icingadb/command/schedule-check   | Allow scheduling host and        |
|                                   | service checks.                  |
+-----------------------------------+----------------------------------+
| icingadb/command/schedule-check   | Allow scheduling host and        |
| /active-only                      | service checks. (Only on objects |
|                                   | with active checks enabled)      |
+-----------------------------------+----------------------------------+
| icingadb/command/acknowledge-pr   | Allow acknowledging host and     |
| oblem                             | service problems.                |
+-----------------------------------+----------------------------------+
| icingadb/command/remove-acknowl   | Allow removing problem           |
| edgement                          | acknowledgements.                |
+-----------------------------------+----------------------------------+
| icingadb/command/comment/*        | Allow adding and deleting host   |
|                                   | and service comments.            |
+-----------------------------------+----------------------------------+
| icingadb/command/comment/add      | Allow commenting on hosts and    |
|                                   | services.                        |
+-----------------------------------+----------------------------------+
| icingadb/command/comment/delete   | Allow deleting host and service  |
|                                   | comments.                        |
+-----------------------------------+----------------------------------+
| icingadb/command/downtime/*       | Allow scheduling and deleting    |
|                                   | host and service downtimes.      |
+-----------------------------------+----------------------------------+
| icingadb/command/downtime/sched   | Allow scheduling host and        |
| ule                               | service downtimes.               |
+-----------------------------------+----------------------------------+
| icingadb/command/downtime/delete  | Allow deleting host and service  |
|                                   | downtimes.                       |
+-----------------------------------+----------------------------------+
| icingadb/command/process-check-   | Allow processing host and        |
| result                            | service check results.           |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/instance | Allow processing commands for    |
|                                   | toggling features on an          |
|                                   | instance-wide basis.             |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/object   | Allow processing commands for    |
| /*                                | toggling features on host and    |
|                                   | service objects.                 |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/object   | Allow processing commands for    |
| /active-checks                    | toggling active checks on host   |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/object   | Allow processing commands for    |
| /passive-checks                   | toggling passive checks on host  |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/object   | Allow processing commands for    |
| /notifications                    | toggling notifications on host   |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/object   | Allow processing commands for    |
| /event-handler                    | toggling event handlers on host  |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| icingadb/command/feature/object   | Allow processing commands for    |
| /flap-detection                   | toggling flap detection on host  |
|                                   | and service objects.             |
+-----------------------------------+----------------------------------+
| icingadb/command/send-custom-no   | Allow sending custom             |
| tification                        | notifications for hosts and      |
|                                   | services.                        |
+-----------------------------------+----------------------------------+
| icingadb/object/show-source       | Allow viewing an object's source |
|                                   | data. (May contain sensitive     |
|                                   | data!)                           |
+-----------------------------------+----------------------------------+

.. _monitoring-module-restrictions:

Restrictions
````````````

The Icinga DB module allows for filtering objects:

+---------------------------+---------------------------------------------+
| Keys                      | Restricts                                   |
+===========================+=============================================+
| icingadb/filter/objects   | Applies a filter to all Icinga objects.     |
+---------------------------+---------------------------------------------+
| icingadb/filter/hosts     | Applies a filter to all hosts and services. |
+---------------------------+---------------------------------------------+
| icingadb/filter/services  | Applies a filter to all services.           |
+---------------------------+---------------------------------------------+

.. rubric:: Filter Column Names


The following filter column names are available in filter expressions:

+---------------------------------------+------------------------------+
| Column                                | Description                  |
+=======================================+==============================+
| host.name                             | Filter on host object names. |
+---------------------------------------+------------------------------+
| hostgroup.name                        | Filter on hostgroup object   |
|                                       | names.                       |
+---------------------------------------+------------------------------+
| host.user.name                        | Filter on user names         |
|                                       | related to hosts.            |
+---------------------------------------+------------------------------+
| host.usergroup.name                   | Filter on usergroup names    |
|                                       | related to hosts.            |
+---------------------------------------+------------------------------+
| service.name                          | Filter on service object     |
|                                       | names.                       |
+---------------------------------------+------------------------------+
| servicegroup.name                     | Filter on servicegroup       |
|                                       | object names.                |
+---------------------------------------+------------------------------+
| service.user.name                     | Filter on user names         |
|                                       | related to services.         |
+---------------------------------------+------------------------------+
| service.usergroup.name                | Filter on usergroup names    |
|                                       | related to services.         |
+---------------------------------------+------------------------------+
| all custom variables prefixed with    | Filter on specified custom   |
| ``host.vars`` or ``service.vars``     | variables.                   |
+---------------------------------------+------------------------------+
