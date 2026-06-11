
Adding a second Satellite (we'll call the Satellite `acmesatellite2`), for `tenant_A`,
in an existing Icinga2 zone, so to create an High Availability configuration
with the existing Satellite `acmesatellite`, takes few steps.

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
   as value for the `icinga2_zone` attribute within :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite2.conf`.
