The Shutdown Manager GUI
~~~~~~~~~~~~~~~~~~~~~~~~

The Shutdown Manager GUI allows to configure the Shutdown Manager and
manage all its components, without the need to access the CLI.

The Shutdown Definition
```````````````````````

A shutdown definition is used to determine a condition on a host, that
when met will start the shutdown process on a host or a group of
hosts; it is built using the same parameters used in the :ref:`same
parameters <shutdownmanager-cli-create>`, with the **Status** as the
**shutdown condition**.

This page contains a list of all the existing shutdown definitions,
along with the Shutdown groups on which each definition will
operate. A click on the groups’ number will open the :ref:`Shutdown
Groups tab <shutdownmanager-gui-group>`, where it is possible to see
and edit each group.

In the Action column appears a *Shutdown* button: provided that users
have the necessary permissions to start and execute the shutdown
process, they will be able to click on the ``red`` *shutdown* button,
otherwise the button will be grey and not clickable.

Upon clicking on the *Shutdown* button in the **Action** column, a
confirmation panel will appear, and a click on the red **Shutdown**
button in the right-hand side panel will execute the shutdown on the
hosts in the first group, followed by the other groups in the Shutdown
Definition, if there are more.

To allow users to execute the shutdown, there is a **new permission**
called ``shutdownmanager/api/trigger-shutdown-definition`` for the
*Shutdown Manager* module. In :menuselection:`Configuration -->
Authentication --> Roles` (see :ref:`Authentication Roles <auth-roles>`), assign the
permission to a new or existing role, then add the user to that role.

New Shutdown definitions can be created by clicking on *Add*. In the
form, provide the name for the new definition, the hosts or groups that
will be interested by the definition and by the condition that must be
met to invoke the shutdown.

.. _shutdownmanager-gui-group:

The Shutdown Group
``````````````````

The Shutdown Group tab contains all the groups of hosts that have been
created. For each group a number of information is shown:

* the associated shutdown definition ID, which is clickable and would
  open the corresponding Shutdown Definition tab
* the order in which the group is processed within the Shutdown
  Definition
* the timeout before the definition operates
* the monitoring view: when clicked, in the right-hand side panel will
  appear the detailed list of the hosts in the group along with a
  number of details.

New Shutdown groups can be created by clicking on *Add*.

The Shutdown Command
````````````````````

A Shutdown Command is the actual command that will be issued on the
hosts or groups when the Shutdown Manager is invoked on a group of
hosts. This page shows for each command the given name and if it should
be run on the master (*0*) or on the agent (*1*). The same information
must be provided when adding a new command.

.. warning:: Once you finish configuring the Shutdown Command objects,
   **you need to deploy the Director configuration** in order to execute
   the Commands.

The Shutdown Output
```````````````````

In this tab it is possible to check in real time the log files produced
during the various shutdown processes. The output includes the timestamp
of the shutdown process, the result of the Shutdown Command and error
messages if the shutdown process failed on some hosts.
