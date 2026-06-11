
.. _add-service-to-satellite-target:

Adding a Service to the Satellite Target
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adding a `systemd service` to ``neteye-satellite.target`` (see :ref:`neteye-satellite-services`),
can be useful in the scenario where a custom systemd service needs to be managed together
with the other services of the Satellite.

The main use case for this necessity is that the Satellite admins want that a service that they created
will be always started automatically when the Satellite Node reboots.

To attach a new `systemd service` to the Satellite Target you can use the command
``neteye satellite service add``. For example, if you want to add the service
``telegraf-local@my_custom_instance.service`` to the ``neteye-satellite.target`` you can execute:

.. code:: bash

   sat# neteye satellite service add telegraf-local@my_custom_instance

.. warning:: The service name passed to the ``neteye satellite service add`` command
   must not contain the ``.service`` suffix, which would lead to an incorrect configuration.

The command ``neteye satellite service remove`` allows instead to remove a service from the Satellite Target.
Removing a service from the Satellite Target can be useful if you previously added a custom service by mistake and
**must not be used to remove a NetEye service** from the Satellite Target.

For example, to remove ``telegraf-local@my_custom_instance.service`` from the ``neteye-satellite.target`` you can
execute:

.. code:: bash

   sat# neteye satellite service remove telegraf-local@my_custom_instance

We remind you that you can verify which services are attached to the Satellite Target with the command:

.. code:: bash

   sat# systemctl list-dependencies neteye-satellite.target

Icinga 2 advanced configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can have a look at :ref:`neteye-second-satellite-conf` for more advanced configurations of Icinga 2
in the |ne| Satellite Nodes.
