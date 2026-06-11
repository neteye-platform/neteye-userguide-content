A **Satellite** is a |ne| instance which depends on a main |ne|
installation, the **Master**, and carries out tasks such as:

* execute :ref:`Icinga 2 <active-monitoring>` checks and forward results to the Master
* collect logs and forward them to the Master
* forward data through :ref:`NATS <data-gathering-in-neteye-satellites>`
* collect data through :ref:`Tornado Collectors <tornado-collectors-overview>` and forward them
  to the Master to be processed by Tornado

The remainder of this section will list the prerequisites for
Satellites, then lead you through the steps needed to configure a new
|ne| Satellite.

For further information about |ne| Satellites, refer to Section :ref:`master-satellite-arch`.

.. _neteye-satellite-configuration:

Prerequisites
~~~~~~~~~~~~~

The following list contains *hard* prerequisites that **must** be
satisfied for proper Satellite operating.

* Both the Master and the Satellite must be equipped with the **same NetEye
  version**

* Satellites can be arranged in **Tenants**

* Ensure that the :ref:`Networking Requirements
  <table-cluster-tcp-communication-req>` for NATS Leaf Nodes are
  satisfied

* The Satellite must be able to reach the |ne| repositories at the URL
  https://repo.wuerth-phoenix.com/. Due to infrastructure limitations,
  in some cases a Satellite node can't access those.
  In order to check this, run the following command on your Satellite:

  .. code:: bash

     curl -I https://repo.wuerth-phoenix.com/rhel8/neteye-$DNF0

  In case the status code received in the output is other than ``200 OK``,
  please open a ticket on the |support|_.

* The NATS connection between Master and Satellite is always initiated
  by the **Satellite**, not by the Master

* If you are in a |ne| cluster environment, check that all resource
  are in **started** status before proceeding with the Satellite
  configuration procedure

* Never run the `neteye install` command on Satellite Nodes,
  because this would remove all Satellite configuration. You will
  therefore end up with a |ne| **Single Node** instead!

* The |ne| Tenant which the Satellite belongs to must be present on the NetEye Master.
  Please refer to :ref:`tenants-configuration` for more information on how to create it.

.. _neteye-satellite-conf:

Configuration of a Satellite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The configuration of a Satellite is carried out in two phases. **Phase
one** consists of the basic networking setup, that can be carried out by
following steps **1** to **4** (i.e., :ref:`Part 1
<ne-setup-part-one>`) of System Setup. **Phase two** consists of the
remainder of this section.

We will use the following notation when configuring a
|ne| Satellite.

* the domain is **example.com**
* the Tenant is called **tenant_A**
* the Satellite is called *acmesatellite*

This notation will be used also for the
:ref:`neteye-second-satellite-conf` and whenever a |ne| Satellite
configuration is mentioned.

In order to create a new Satellite (`acmesatellite`), **on the
Master** create the configuration file
:file:`/etc/neteye-satellite.d/tenant_A/acmesatellite.conf` (the folder
:file:`/etc/neteye-satellite.d/tenant_A/` is already present if you
configured `tenant_a` as explained in :ref:`tenants-configuration`).

.. note:: The basename of the file, without the trailing ``.conf``
   (i.e., `acmesatellite`), will be used as Satellite unique
   identifier in the same Tenant.

In single Tenant environments where it is not expected to introduce multi-tenancy in the near
future, you may want to create the Satellite in the *master Tenant*. Satellites belonging
to the *master Tenant* belong to the same Tenant of the |ne| Master.

.. topic:: Satellites naming and other naming conventions

   Satellites must satisfy the following requirements:

   * Satellite name must match the following regex  ``/^[a-zA-Z0-9]{1,32}$/``, i.e. it must contain only alphanumeric characters
     and must contain between 1 and 32 characters
   * Should not contain the ``master`` string
   * Satellite name **must be unique** within a Tenant, but Satellites in different Tenants may have the same name
   * For icinga2_zone only: match the following regex  ``/^[[:alnum:][:blank:]_-]+$/``, i.e., it must contain only alphanumeric
     characters, underscores, dashes, and white spaces

   It is also suggested to use a meaningful name for each Satellite
   and each tenant you want to configure.

   While Tenants and Satellites can be renamed, the procedure is not
   automatic and requires manual intervention.

   The following rules are always applied and may influence the final
   value of the configuration option.

   * If `icinga2_zone` is not defined, then default
     <tenant>_<satellite name> will be used as zone name
   * If the user specifies the `icinga2_zone` attribute, then <tenant> will be prepended
   * If the user also specifies the attribute
     `icinga2_tenant_in_zone_name` with value `false`, then <tenant>
     is not prepended
   * If the Tenant is the special Tenant **master**, then <tenant> is
     never prepended to the zone name

The configuration, including all optional parameters, should look
similar to this excerpt.

.. code:: json

   {
     "fqdn": "acmesatellite.example.com",
     "name": "acmesatellite",
     "ssh_port": "22",
     "ssh_enabled": true,
     "icinga2_zone": "acme_zone",
     "icinga2_wait_for_satellite_connection": false,
     "icinga2_tenant_in_zone_name": true,
     "proxy": {
       "ssl_protocol": "TLSv1.2 TLSv1.3",
       "ssl_cipher_suite": "HIGH:!aNULL:!MD5:!3DES"
     }
   }

.. _neteye-satellite-config-params:

The configuration file of the Satellite must contain the following attributes:

* **fqdn**: the Satellite's Fully Qualified Domain Name

* **name**: the Satellite name, which **must** coincide with the
  configuration file name

* **ssh_port**: the port to use for SSH communication from Master to
  Satellite

  .. hint:: You can specify a different port from default *22* in case of custom SSH
     configurations.

* **ssh_enabled**: if set to *true*, SSH connections from Master to
  the Satellite can be established, otherwise they are not allowed and
  Satellite's configuration files must be manually copied from Master

* **icinga2_zone**: the Satellite's Icinga 2 high availability zone. This parameter is
  optional and default value is *<tenant>_<satellite name>*

* **icinga2_wait_for_satellite_connection**: if set to *false* the
  Satellite will wait for Master to open the connection. This
  parameter is optional and default value is *true*.

* **icinga2_tenant_in_zone_name**: if set to *false*, the Tenant's
  name is not prepended to the Icinga 2 zone name. This parameter is
  optional and default value is *true*.

  .. note:: This parameter should be used only for existing
     multi-tenant installations. For this reason, its usage is
     strongly discouraged for new installations. If a multi-tenant
     installation is not required, please use the special Tenant
     *master* instead.

* **proxy.ssl_protocol**: the set of protocols allowed in NGINX. This
  parameter is optional and its default value is *TLSv1.2 TLSv1.3*.
  Change this value to either improve security or to allow
  older protocols (for backward compatibility only).

* **proxy.ssl_cipher_suite**: the cypher suite allowed in NGINX. This
  parameter is optional and its default value is
  *HIGH:!aNULL:!MD5:!3DES*. Change this value to either
  improve security or to allow older cyphers (for backward
  compatibility only).

* **icinga2_endpoint_log_duration**: the amount of time for which
  replay logs are kept on connection loss. It corresponds to
  **log_duration** when defining Icinga 2 endpoints, as described in
  `Icinga 2 official documentation
  <https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#endpoint>`__
  This parameter is optional and, if not set, it will take Icinga 2
  defaults (i.e., **1d** or **86400s**).

* **ntp_servers**: a list of NTP servers to be used by the Satellite.
  Example: ``["1.my.ntp.org", "2my.ntp.org"]``
  This parameter is optional and, if not set, it will
  ensure the default NTP RHEL pool is used.

.. note:: Remember to append the FQDN of the Satellite in :file:`/etc/hosts`.
   If you are in a cluster environment you must change the :file:`/etc/hosts`
   on each node of the cluster.

If you are installing a Satellite within a cluster, run the following command
to synchronise the files
:file:`/etc/neteye-satellites.d/*` and :file:`/etc/neteye-cluster` on
all cluster nodes:

.. code:: bash

   cluster# neteye config cluster sync

.. _neteye-satellite-config-create:

Generate the Satellite Configuration Archive and Configure the Master
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To generate the configuration files for the `acmesatellite` Satellite
and to reconfigure Master services, run on the Master the command
below, which generates all the required configuration files for the
Satellite, stored in file
:file:`/root/satellite-setup/config/<neteye_release>/tenant_A-acmesatellite-satellite-config`.

.. warning:: On a cluster, this command will temporarily put all the
   cluster resources in `unmanaged` state.  This means that
   :command:`pcs` will not take care of handling clusterized services
   until a valid configuration is successfully created. In case of
   error during the execution of the :command:`neteye satellite config
   create` command the cluster is left in unmanaged state to avoid
   downtimes. If this happens the user is required to:

   - fix the errors
   - run again the command

   .. code:: bash

      master# neteye satellite config create acmesatellite


The command executes the Master autosetup scripts located in :file:`/usr/share/neteye/secure_install_satellite/master/`,
automatically reconfiguring services to allow the interaction with the Satellite.

For example, the **NATS Server** is reconfigured to authorize leaf connections
from `acmesatellite`, while streams coming from the Satellite are exported in order
to be accessible from Tornado or Telegraf consumers.

In case the same name is used for more than one Satellite in different Tenants, then the **--tenant**
switch must be used to specify the desired Tenant.

.. code:: bash

   master# neteye satellite config create acmesatellite --tenant tenant_A

.. note:: The command `neteye satellite config create` computes the resulting Icinga 2 Zone name
   at run-time, also validating the name in the process.

   The resulting Zone, which can be different from the one specified
   via the `icinga2_zone` attribute, must be unique across all Tenants.
   In case the property is not satisfied, the `neteye satellite config create`
   command triggers an error, stopping the Satellite configuration.

The **Telegraf** local Consumer  ``telegraf-local@neteye_consumer_telegraf_metrics`` is also automatically reconfigured
for each Tenant, to `consume` metrics coming from the Satellites through NATS and to write them to InfluxDB.

.. note:: If you are in a cluster environment, an instance of Telegraf local consumer is
   started on each node of the cluster, to exploit the NATS built-in load balancing feature
   called `distributed queue`. For more information about this feature, see the `official
   NATS documentation <https://docs.nats.io/nats-concepts/queue>`__

The command also creates an archive containing all the configuration files, in order to
easily move them to the Satellite. The archive can be found at :file:`/root/satellite-setup/config/<neteye_release>/tenant_A-acmesatellite-satellite-config.tar.gz`

In case you would like to generate the configuration for more than one satellite, you can
specify more than one Satellite name, separated by spaces.

.. code:: bash

   master# neteye satellite config create acmesatellite mysecondsatellite

If specifying multiple satellites some are ambiguous (they are present in more than one tenant),
please consider to split the configuration generation into separate calls, one for each tenant.

Alternatively, configurations can be generated for all the Satellites of all Tenants defined
in :file:`/etc/neteye-satellites.d/` by typing:

.. code:: bash

   master# neteye satellite config create --all

.. _neteye-satellite-config-send:

Synchronize the Satellite Configuration Archive
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Satellite configuration is synchronised automatically if the attribute
``ssh_enabled`` is set to **TRUE**, using the port defined by the
attribute ``ssh_port`` or the default SSH port (**22**) otherwise..

.. note:: If SSH is not enabled, the configuration archive must be
   **manually copied** before proceeding, see below.

To synchronize the configuration files between the Master and the `acmesatellite`
Satellite, provided ``ssh_enabled`` is set to **TRUE**, run the following command on the Master:

.. code:: bash

   master# neteye satellite config send acmesatellite

In case the same name is used for more than one satellite in different
Tenants, then the `--tenant` option has to be used to specify the
destination Tenant.

.. code:: bash

   master# neteye satellite config send acmesatellite --tenant tenant_A

The command uses the unique ID of the Satellite to retrieve the connection attributes from
the Satellite configuration file :file:`/etc/neteye-satellites.d/tenant_A/acmesatellite.conf`, and
uses them to send the archive `tenant_A-acmesatellite-satellite-config.tar.gz` to the Satellite.

Alternatively, configuration archives can be sent to all Satellites defined
in :file:`/etc/neteye-satellites.d/` by typing:

.. code:: bash

   master# neteye satellite config send --all

The configuration archives for each Satellite belonging to a specific Tenant, will be sent to the
related Satellite using the following command:

.. code:: bash

   master# neteye satellite config send --tenant tenant_A

.. _neteye-satellite-setup:

Satellite Setup
~~~~~~~~~~~~~~~

After the configuration has been generated and sent to a Satellite,
use the following command on the **Satellite** itself to complete its
configuration:

.. code:: bash

   sat# neteye satellite install

This command performs three actions:

- Copies the configuration files in the correct places overriding current configurations, if any.
- Creates a backup of the configuration for future use in
  :file:`/root/satellite-setup/config/<neteye_release>/satellite-config-backup-<timestamp>/`
- Executes autosetup scripts located in :file:`/usr/share/neteye/secure_install_satellite/satellite/`

To execute this command the configuration archive must be located in
:file:`/root/satellite-setup/config/<neteye_release>/satellite-config.tar.gz`.
Use :command:`neteye satellite config send` command or copy the archive manually
if no SSH connection is available.

.. note:: Configuration provided by the Master is not user customizable: any change will be
   overwritten by the new configuration when running :command:`neteye satellite install`
