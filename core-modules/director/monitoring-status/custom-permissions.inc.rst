.. _monitoringview-module:

Monitoring view
```````````````

The :ref:`Monitoring panels <active-monitoring>` in NetEye are divided
into several sections, each carrying different information.  Access to
the Monitoring View requires the user to have a role with appropriate
permissions in the **Icinga DB** module; a Role with *General Module
Access* usually suffices for this purpose.

The **monitoringview** module introduces a new functionality to
selectively hide or show each section of the host and service pages to a
role - and therefore to a user. You can grant these permissions
in **Configuration > Access Control > Roles** under the *monitoringview*
module; each section as an associated permission with the same name:

-  **Actions**: ``monitoringview/actions``
-  **Check statistics**: ``monitoringview/check-statistics``
-  **Comments**: ``monitoringview/comments``
-  **Custom variables**: ``monitoringview/custom-variables``
-  **Downtimes**: ``monitoringview/downtimes``
-  **Feature Command**: ``monitoringview/feature-command``
-  **Groups**: ``monitoringview/groups``
-  **Notifications**: ``monitoringview/notifications``
-  **Performance Data**: ``monitoringview/performance-data``
-  **Plugin Output**: ``monitoringview/plugin-output``
-  **Services**: ``monitoringview/services``

.. note:: Monitoring Panels have two additional sections,
   ``Performance Graph`` and ``Quick Actions``, which are not managed
   within this module. You can hide or show these sections by modifying
   the corresponding permission in their modules:

-  **Performance Graph**: Already present in ITOA module, this section
   can be hidden or shown by acting on ``general module access`` and
   ``analytics/view-performance-graph`` permission from analytic module.
-  **Quick Actions**: It is a Icinga DB module permission that can be
   used to hide complete or specific command with the
   ``icingadb/command/*`` permission. This Quick Actions section has
   commands like ``check now``, ``Comment``, ``Notification``,
   ``Downtime``.
