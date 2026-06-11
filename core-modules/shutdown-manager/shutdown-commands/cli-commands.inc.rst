
Shutdown Management Configuration CLI Commands
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use the Shutdown Manager directly from the shell with the
*icingacli* command.

Using the Shutdown Manager’s CLI commands, you can perform `create,
edit, delete and list` actions on the following *shutdownmanager*
objects:

#. **Shutdown Definition:** A :term:`Shutdown Definition`
   that specifies how to shut down multiple computers when a condition
   is met.
#. **Shutdown Group:** A :term:`Shutdown Group`
   which should all be shut down at the same time.

Below you can find detailed descriptions of the available commands and
their parameters.

Shutdown Definition Commands
````````````````````````````

.. _shutdownmanager-cli-create:

Create
++++++

The *create* command lets you construct a new shutdown definition. It
requires a name and a **Shutdown Condition**, expressed as a :term:`Host State` or a
:term:`Service State` depending on the command’s parameters.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowndefinition create [parameters]

**Available Parameters:**

:*–name*: (mandatory) The name of the shutdown definition to be
	  created.
:*–host-name*: (mandatory) The name of the host whose status will
	       trigger the shutdown, or which is running the service
	       that will trigger the shutdown.
:*–service-description*: (optional) If this parameter is empty, then
			 the host’s status will be compared to the
			 target status. If instead it is set to the
			 name of a valid service on the host, then the
			 service’s status will be used.
:*–status*: (mandatory) The monitoring status that will trigger the
	    shutdown procedure when it matches the host or service
	    status.

.. _shutdownmanager-cli-edit:

Edit
++++

The *edit* command lets you change one or more of the values for the
fields in an existing shutdown definition using the :ref:`same
parameters <shutdownmanager-cli-create>` as the ``shutdowndefinition
create`` command above.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowndefinition edit [parameters]

.. _shutdownmanager-cli-list:

List
++++

The *list* command lets you see a list of all existing shutdown
definitions in JSON format.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowndefinition list

.. _shutdownmanager-cli-delete:

Delete
++++++

The *delete* command lets you remove an existing shutdown definition
given that definition’s ID, which you can obtain from the *list*
command.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowndefinition delete [parameters]

**Available Parameters:**

:*–id*: (mandatory) The ID of the definition to delete

Shutdown of a definition
++++++++++++++++++++++++

The *shutdowndefinition* command lets you trigger the shutdown of a
pre-configured shutdown definition.

Once a shutdown group is triggered, a timer will count down while the
hosts in that shutdown group are powering down. Once the timeout period
has expired, the subsequent shutdown group will be triggered and its
timer set, until no more shutdown groups remain.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdown shutdowndefinition [parameters]

**Available Parameters:**

:*–id*: (mandatory) The ID of the shutdown definition which has to be
	shutdown


Shutdown Group Commands
```````````````````````

.. _shutdownmanager-cli-group-create:

Create
++++++

The *create* command lets you construct a new shutdown group. It
requires a name, a set of monitored objects, a shutdown definition to
belong to, and an ordering relative to other groups.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowngroup create [parameters]

**Available Parameters:**


:*–name*: (mandatory) The name of the shutdown group to be created.

:*–filter*: (mandatory) A monitoring filter that will return a set of
	    monitored objects to be included in this new group.

:*–shutdown-definition-id*: (mandatory) The ID of a shutdown
			    definition (obtainable via the *list*
			    command) which this new group will then
			    belong to.

:*–timeout*: (mandatory) The number of seconds after which the
	     shutdown process for the next group will be initiated.

:*–group-order*: (optional) A number representing the order of this
		 group relative to other groups in the shutdown
		 definition.  Lower-numbered groups are shut down
		 before higher-numbered groups, and any groups having
		 the same order number in the shutdown definition will
		 be shut down in a random order relative to
		 themselves. If no group order parameter is specified,
		 then the value will default to the next highest
		 available number (for instance, if you have only one
		 group with order **3** in the definition, the new
		 group will have order **4**). If no groups yet exist
		 in the shutdown definition, it will be set to ``1``.

.. _shutdownmanager-cli-group-edit:

Edit
++++

The *edit* command lets you change one or more values for the fields
in an existing shutdown group using the :ref:`same parameters
<shutdownmanager-cli-create>` as the ``shutdowngroup create`` command
above.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowngroup edit [parameters]

.. _shutdownmanager-cli-group-list:

List
++++

The *list* command lets you see all existing shutdown groups in JSON
format.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowngroup list

**Available Parameters:** *None*


.. _shutdownmanager-cli-group-delete:

Delete
++++++

The *delete* command lets you remove an existing shutdown group given
that group’s ID, which you can obtain from the *list* command.

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowngroup delete [parameters]

**Available Parameters:**

:*–id*: (mandatory) The ID of the group to delete

.. _shutdownmanager-cli-group-listhosts:

Listhosts
+++++++++

The *listhosts* command lets you see all hosts that belong to a shutdown
group in JSON format. Each entry in the output list represents a host
and contains:

* The host name
* The host address
* The ID of the shutdown command associated with the host (**null** if
  the host is not associated with any shutdown command)

**Usage:**

.. code:: bash

   # icingacli shutdownmanager shutdowngroup listhosts [parameters]

**Available Parameters:**

:*–id*: (mandatory) The ID of the shutdown group whose hosts are to be
	displayed
