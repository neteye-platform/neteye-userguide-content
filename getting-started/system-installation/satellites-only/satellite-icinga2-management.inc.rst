
.. note:: It is important to know that the term
   *multiple Satellites* refers to a maximum of two Satellites in the same Icinga 2 zone.

Adding a Second Satellite in an Existing Icinga 2 Zone
``````````````````````````````````````````````````````
Adding a second Satellite (we'll call the Satellite `acmesatellite2`) for `tenant_A`
in an existing Icinga 2 zone, in order to create a High Availability configuration
with the existing Satellite `acmesatellite`, takes just a few steps.

To start, write the configuration file :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite2.conf`
on the **Master**, in tenant folder `tenant_A`, for Satellite `acmesatellite2`.
Satellite `acmesatellite2` must be defined having the same `icinga2_zone` as for `acmesatellite`.

Once the `acmesatellite2` configuration file has been prepared, run commands

.. code:: bash

   neteye satellite config create

and

.. code:: bash

    neteye satellite config send

on the **Master**, for both Satellites `acmesatellite` and `acmesatellite2`, to create
the new configurations and to send them to the corresponding Satellites.

Once done, execute

.. code:: bash

   neteye satellite install

on both **Satellites**. See the :ref:`Satellite Configuration <neteye-satellite-configuration>`
for details.

.. note:: If `icinga2_zone` is not defined in Satellite
   :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite.conf`, to add
   a second Satellite in an existing zone, use the name of the first Satellite (`acmesatellite`)
   as the value for the `icinga2_zone` attribute within :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite2.conf`.


Adding Two Satellites in the Same Icinga 2 Zone
```````````````````````````````````````````````

When you need to add two new Satellites in the same Icinga 2 zone at the same stage of configuration,
add both Satellite nodes first and then generate the configuration for both
of them in a single workflow. This ensures that the zone configuration is
created consistently and distributed only after both Satellites are known
to NetEye.

The steps are:

#. Add the first Satellite, at this stage by creating
   and configuring the :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite.conf` file.

#. Add the second Satellite by creating and configuring :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite2.conf` file before applying
   or sending the Satellite configuration. Define the same Icinga 2 zone that was specified in the
   :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite.conf`.

#. Apply the Satellite configuration for both nodes at once:

   .. code-block:: bash

      neteye satellite config create <first-satellite-fqdn> <second-satellite-fqdn>

#. Send the configuration to both Satellites at once:

   .. code-block:: bash

      neteye satellite config send <first-satellite-fqdn> <second-satellite-fqdn>

#. Verify that both Satellites have received the configuration and that the
   Icinga 2 services are running correctly on both nodes.

.. note::

   Do not run ``neteye satellite config create`` and
   ``neteye satellite config send`` after adding only the first Satellite.
   In this scenario, both Satellites must be added first, and the
   configuration must then be created and sent for both Satellites together.


For example, if the two new Satellites are ``satellite-02.example.com`` and
``satellite-03.example.com``, run:

.. code-block:: bash

   neteye satellite config create satellite-02.example.com satellite-03.example.com
   neteye satellite config send satellite-02.example.com satellite-03.example.com
