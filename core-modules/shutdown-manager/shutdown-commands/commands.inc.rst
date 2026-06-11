
.. _monitor-sm-conf:

The Shutdown Command
~~~~~~~~~~~~~~~~~~~~

A so-called ``shutdown command`` must be associated with each monitored
Host that should be shut down by the module. This is the exact command
which will be executed by the Shutdown Manager module on the host to
shut it down. The link between monitored host and shutdown command is
created by assigning a dedicated **Custom Property** in the host’s
configuration form in Icinga Director.

To actually trigger the shutdown of a host, you will need to go to the
Shutdown Manager module and invoke the command from there.

Each ``shutdown command`` is run using *icingacli*.

Shutdown commands return the result of the operation as a JSON
structure. For *list* commands, this structure will be an array of the
returned objects. For the other commands (*create*, *delete*, etc.) it
will contain:

* *result:* Whether the command succeeded or failed (‘ok’ or ‘failed’)
* *message:* A text-based confirmation message
* *info:* Detailed information on relevant parameters

.. warning:: Once you finish configuring the Shutdown Command objects,
   **you need to deploy the Director configuration** in order to execute
   the Commands.

.. _shutdownmanager-create:

Create
``````

The *create* command lets you add a new shutdown command which you can
then apply to the *Shutdown command* custom property of the host or host
group in Director.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowncommand create [parameters]

**Available Parameters:**

:*-name*: (mandatory) Name of the command.

:*-command*: (mandatory) The command field must consist of a JSON
             Array that represents the command and its associated
             parameters needed to correctly shut down the host. If the
             command requires host-related parameters, the user can
             specify them using the pattern *$<property\>$*, and they
             will be replaced by the Shutdown Manager (if correctly
             specified in the host definition). Currently the
             supported parameters are:

	     * *$host$*
	     * *$hostAddress$*
	     * *$hostAddress6$*
	     * *$hostAlias$*
	     * *$hostIpv4$*
	     * *$objectType$*
	     * *$hostName$*

:*–run-on-agent*: (mandatory) Determines whether the command should be
                  executed on the master, or on an Icinga agent. This
                  will only succeed if the Icinga agent is running on
                  the host, and is connected to either the master or a
                  satellite when the shutdown command is
                  triggered. The value to pass is either *0* (false,
                  i.e. run on the master) or *1* (true, i.e. run on
                  the agent).

.. _shutdownmanager-edit:

Edit
````

The *edit* command lets you change one or more values on an existing
shutdown command using the :ref:`same parameters
<shutdownmanager-create>` as the ``shutdowncommand create`` command
above.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowncommand edit [parameters]

.. _shutdownmanager-list:

List
````

The *list* command lets you see a list of all existing shutdown commands
in JSON format.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowncommand list

**Available Parameters:**

:*–id*: (mandatory) The ID of the definition to list


.. _shutdownmanager-delete:

Delete
``````

The *delete* command lets you remove a shutdown command you have
created, provided that it is **not in use** on any hosts. It requires
the shutdown command’s ID, which you can obtain from the *list* command.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowncommand delete [parameters]

**Available Parameters:**

:*–id*: (mandatory) The ID of the shutdown command to delete


.. _shutdownmanager-group-host:

Triggering the Shutdown of All Hosts in a Shutdown Group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All hosts in a Shutdown Group can be shut down directly using this
dedicated **icingacli** command:

.. code:: bash

   icingacli shutdownmanager shutdown shutdowngroup --id <ID>

The ``shutdown shutdowngroup`` command requires the group’s ID, which
you can obtain by using the :ref:`group’s list command
<shutdownmanager-list>`.  During command execution, all hosts in that
group will be sent their :ref:`individual shutdown command
<shutdownmanager-single-host>`.

The ``shutdown shutdowngroup`` **icingacli** command will return one of
the following values:

* **0** : Ok
* **1** : The mandatory *ID* parameter is missing
* **2** : The group with the given ID does not exist
* **3** : At least one error occurred while sending the shutdown
  command to the hosts in the group

.. _shutdownmanager-single-host:

Triggering the Shutdown of a Single Host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A single host can be shut down directly using the dedicated
**icingacli** command::

  icingacli shutdownmanager shutdown host --host-name <host name>

The ``shutdown host`` command requires a parameter for the host name.
During command execution, a check is performed which validates that the
specified host exists. In addition, the command itself verifies whether
a shutdown command has been assigned to the given host.

The Shutdown Manager will then substitute the supported macro parameters
and communicate to **Icinga** the resulting shutdown command. **Icinga**
will take care of executing the shutdown command.

The ``shutdown host`` **icingacli** command will return one of the
following values:

* **0** : Ok
* **1** : The mandatory *host-name* parameter is missing
* **2** : No hosts or more than one host was passed as the *host-name*
  parameter
* **3** : An error occurred while sending the shutdown command to
  **Icinga**
