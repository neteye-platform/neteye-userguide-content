.. _slm-event-adjustment-cli:

The Event Adjustment CLI Command
++++++++++++++++++++++++++++++++

The event adjustment feature is also available through the CLI.
Below you can find detailed descriptions of the available commands and
their parameters.

.. note:: All timestamps in the commands below must be in the format
   **YYYY-MM-DD hh:mm:ss** and be expressed in your local timezone as
   :ref:`set in PHP during the initial configuration <neteye-initial-conf>`.

.. rubric:: Create


The *create* command lets you create a new event adjustment for a
particular :term:`Monitored Object` (host or service).

**Usage:**

.. code:: bash

   # icingacli slm adjustments create [parameters]

**Available Parameters:**

:*--host-name*: (mandatory) Name of the host to which attach the event
:*--service-description*: (optional) Name of the service, running on
			 host *$host-name*, to which to attach the
			 event
:*--description*: (mandatory) Title for the event
:*--start*: (mandatory) Timestamp for the starting point of the event
:*--end*: (mandatory) Timestamp for the ending point of the event
:*--event-type*: (optional) State for the event; if not explicitly
		 defined it will be automatically set to
		 **downtime**. The event type must be one of the values
		 in the table above that is available for the
		 monitored object passed in the *$host-name* or
		 *$service-description* parameters.

**Example**

I want to create an event adjustment for the host “my-host” to specify
that this host was up and running yesterday night from 1 AM to 3 AM.

I then execute the following command::

  icingacli slm adjustments create --host-name="my-host" --description="Wrong host state" --start="2019-12-18 01:00:00"  --end="2019-12-18 03:00:00" --event-type="up"

.. rubric:: List


The *list* command lets you see existing event adjustments. The output
will be a JSON object.

**Usage:**

.. code:: bash

   # icingacli slm adjustments list

.. rubric:: Edit


The *edit* command lets you alter the starting or ending time of an
existing event adjustment given that adjustment’s ID, which you can
obtain from the *list* command. Note that you cannot change the host or
service name filters this way - you will have to delete and recreate the
event adjustment. Also, if neither the *–start* nor the *–end*
parameters are included, the event adjustment will not be changed.

**Usage:**

.. code:: bash

   # icingacli slm adjustments edit [parameters]

**Available Parameters:**

:*--id*: (mandatory) The ID of the adjustment to change
:*--start*: (optional) Timestamp for the starting point of the event
:*--end*: (optional) Timestamp for the ending point of the event
:*--event-type*: (optional) State for the event. The event type must be
	       one of the values in :numref:`table-event-types` above that are available
	       for the monitored object pointed to in the event
	       adjustment.

.. rubric:: Delete


The *delete* command lets you delete an existing event adjustment given
that adjustment’s ID, which you can obtain from the *list* command.

**Usage:**

.. code:: bash

   # icingacli slm adjustments delete [parameters]

**Available Parameters:**

:*--id*: (mandatory) The ID of the adjustment to change
