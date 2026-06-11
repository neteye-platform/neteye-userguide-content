
.. _add-host:

Adding a Host
`````````````

The New Host Panel (:numref:`figure-add-host`) is similar to the
template panels, and is accessible from **Director > Host objects >
Hosts**. Each row in the panel represents a single monitored host with
both host name and IP address. Clicking on the host name shows the
Host Configuration panel for that host to the right, and the “Add”
action brings up an empty Host Configuration panel.

.. _figure-add-host:

.. figure::  /core-modules/director/img/new-host02.png
   :alt: Adding a new host

   Adding a new host

Like the template panel, there are tabs for **Preview**, **History**
and **Agent**. In addition, there is a **Services** tab which shows
all services assigned to that host, organized by inheritance and
service set. Below the tabs is a “Show” action which takes you
directly to the host object's `monitoring panel` (i.e.,g click on the
host under :menuselection:`Icinga Director --> Hosts --> Hosts`)


The following fields are important:

* **Hostname:** This should be the host's fully qualified domain name.
* **Imports:** The host template(s) to inherit from.
* **Display name:** A more friendly name shown in monitoring panels
  which does not have to be a FQDN.
* **Host address:** The host’s IP address.
* **Groups:** A drop down menu to assign this host to a defined host
  group.
* **Inherited groups**, **Applied groups**: Assigned host groups,
  organized by how the group was assigned.
* **Disabled:** Temporarily remove a host from monitoring, without
  deleting its configuration.
* **Custom properties:** Fields defined for host templates, with the
  ability to select a value of the pre-defined type.

The remaining fields should be set on one of the host's parent
templates.

.. Note:: You cannot create a host that does not inherit from at least
   one host template.

.. _add-command-check:

Adding a Command Check
``````````````````````

The New Command Panel (:numref:`figure-add-command`, **Director >
Commands > Commands**) displays one command per line, including the
command name and the check command to be used (without
arguments). While you can always click on the name of each field in
this panel to see a description at the bottom of the web page, here is
a quick summary:

-  **Command Type:** This is the same list as in the :ref:`command
   template panel <command_templates>` section.
-  **Command Name:** The reference name used to assign this check
   command to a service.
-  **Imports:** The parent command template(s). Unlike hosts and
   services, this is optional.
-  **Command:** The actual check command to use, without arguments.
-  **Timeout:** A timeout that will override an inherited timeout.
-  **Disabled:** Disabled commands cannot be assigned.

.. _figure-add-command:

.. figure::  /core-modules/director/img/new-command01.png
   :alt: Adding a new command

   Adding a new command

As with the command template panel, there is an **Arguments** tab
(:numref:`figure-add-command-argument`) that allows you to create
parameter lists for the check commands either from scratch, or by
overriding defaults from an inherited command template. You must
create a separate entry for each parameter, which will then appear in
the table below. To edit an existing parameter, simply click anywhere
on its row in that list.

.. _figure-add-command-argument:

.. figure::  /core-modules/director/img/new-command03.png
   :alt: Adding command arguments

   Adding command arguments

For example, if when executing the check in a shell you need to use a
parameter like “-C” with a given value, you will need to add it as an
argument. All such arguments need to be listed in the Arguments table.
For an argument's “Value” parameter, you can enter either a system
variable or a custom variable, both of which are indicated by a ‘$' both
before and after the variable name. This allows you to parameterize
arguments across multiple host or service templates, including with any
“Custom properties” fields you have created for those templates. This
way you could parameterize for instance the following very common values
at the Service level, and later change them all simultaneously if
desired:

* Credentials such as SNMP community strings, or usernames and
  passwords for SQL login
* Common warning and critical thresholds
* Addresses and port numbers
* Units

For further details about command arguments, please see the `official
documentation
<https://www.icinga.com/docs/icinga2/latest/doc/03-monitoring-basics/#command-arguments>`__.

.. _add-service:

Adding a Service
````````````````

The Service Panel (**Director > Monitored Services > Single Services**)
lists individual services that can be assigned to monitored hosts.

.. _figure-list-services:

.. figure::  /core-modules/director/img/new-service09.png
   :alt: The list of services in the Service Panel

   The list of services in the Service Panel

Click on the “Add” action to display the New Service Panel ((:numref:`figure-add-service`)
where you can create a new service by setting the following fields:

-  **Name:** Give the service a unique name.
-  **Imports:** The parent service template(s).
-  **Host:** The name of at least one host or host template to which
   this service should be applied.
-  **Groups:** The name of one or more service groups to which this
   service should belong.
-  **Disabled:** Whether or not this service can be assigned.

.. _figure-add-service:

.. figure::  /core-modules/director/img/new-service10.png
   :alt: Adding a new service

   Adding a new service

.. note:: You cannot create a service that does not inherit from at
   least one service template.


Host to Service View
````````````````````

When finding your hosts in the Overview, choosing a specific host would open services view of
the host, allowing to see all the services with status associated with it.

In previous NetEye versions, clicking on a specific host in the Overview redirected to the host-details view.

.. note:: You can anytime rollback to the default functionality
   (i.e., host to host details view), by disabling the
   ``host2servicedetailview`` module in NetEye
   (:menuselection:`Configuration --> Modules -->
   host2servicedetailview`)
