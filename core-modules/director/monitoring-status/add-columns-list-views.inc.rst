.. _monitoring-module-add-columns-list-views:

Add Columns to List Views
`````````````````````````

The Icinga DB module provides list views for hosts and services. These
lists only provide the most common columns to reduce the backend query
load.

.. figure:: /core-modules/director/monitoring-status/img/host-list.png
   :alt: Host list

   Host list

The host and service list of the Icinga DB module lets you show/export
additional information per object by using the URL parameter ``columns``.

If you pass this to the host and service list, you'll get an entirely
different view mode in which you have full control over the information displayed.
The parameter accepts a comma separated list of columns.
This list also defines the order in which the columns are shown.

Example for showing the host ``address`` attribute in a host list::

   /neteye/icingadb/hosts?columns=host.address


.. figure:: /core-modules/director/monitoring-status/img/list-hosts-add-columns.png
   :alt: Host list with column parameter specified

   Host list with column parameter specified

Example for multiple columns as comma separated parameter string. This
includes a reference to the Icinga 2 host object custom attribute ``os``
using ``host.vars`` as custom variable identifier::

   /neteye/icingadb/hosts?columns=host.address,host.vars.os

.. figure:: /core-modules/director/monitoring-status/img/list-hosts-add-columns-custom-vars.png
   :alt: Host list with custom vars specified in column parameter

   Host list with custom vars specified in column parameter
