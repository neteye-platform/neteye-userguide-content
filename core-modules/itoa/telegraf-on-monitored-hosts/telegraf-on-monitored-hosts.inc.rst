.. _telegraf-installation:

Telegraf on Monitored Hosts
~~~~~~~~~~~~~~~~~~~~~~~~~~~

On |ne|, Telegraf is installed by default as part of |ne| core. For hosts that don't have
|ne| installed, we provide packages for the most common systems. The version of the Telegraf
on the host should always match the version of |ne| it communicates with. It can be pulled
from the corresponding repository.

.. rubric:: Installing Telegraf on CentOS / Fedora / RedHat

The Package for CentOS, Fedora and RHEL on an x86_64 architecture can be directly installed from the |ne|
repository by adding the file :file:`/etc/yum.repo.d/neteye-telegraf.repo` with the following content:

.. parsed-literal::

    [neteye-telegraf]
    name=NetEye Telegraf Packages
    baseurl= |tg_rh|
    gpgcheck=0
    enabled=1
    priority=1

Then, install Telegraf with the command
:command:`dnf install telegraf`.

.. rubric:: Installing Telegraf on Debian / Ubuntu

To add the Telegraf repository on Ubuntu or Debian systems please create file
:file:`neteye-telegraf.list` in the directory :file:`/etc/apt/sources.list.d/`.

.. parsed-literal::

    deb [trusted=yes] |tg_deb| stable main

Run :command:`apt update` to update the repository data and :command:`apt install telegraf` to
install Telegraf.

.. note::
   In order to verify the repository signature, the `ca-certificates` package must be installed
   on the target machine. This can be achieved with the command :command:`apt install ca-certificates`

.. rubric:: Installing Telegraf on Windows

The files for Windows are hosted on |tg_win|
as a zip file containing the executable and a configuration file.

.. rubric:: Installing Telegraf on Linux

For all other Linux distributions that run x86_64, we provide a :file:`.tar.gz` archive to install. It
can be found on |tg_linux|.

.. rubric:: Installing Telegraf on other Platforms

If you need to install Telegraf on MacOS or on an architecture other than x86_64, please refer to the
`official Telegraf download page <https://portal.influxdata.com/downloads/>`__
under section `Telegraf open source data collector`.

.. _write-data-to-nats-master:

Send metrics directly to Master
```````````````````````````````

|ne| provides a NATS user and its certificates to connect external or
internal Telegraf instances directly to the NATS master instance.

The NATS user for this purpose is `telegraf_wo`, it only has the ability
to publish on subject `telegraf.>` and cannot subscribe to any subject.

The related certificates are located in

.. code-block::

   /neteye/local/telegraf/conf/certs/telegraf_wo.crt.pem
   /neteye/local/telegraf/conf/certs/root-ca.crt
   /neteye/local/telegraf/conf/certs/private/telegraf_wo.key.pem

The NATS server will take care of adding the prefix `master.` to all
messages sent by this user.

All data sent with this user are automatically collected and written to
InfluxDB by a local Telegraf instance using the NATS user `telegraf_ro`.
This user, as opposed to the user used to send the data, can subscribe
to the subject `master.telegraf.>` but cannot publish any message.

To setup a Telegraf agent, please follow the
`official Telegraf documentation <https://docs.influxdata.com/telegraf/>`__.

After setting up a new Telegraf instance, the output section of the configuration
file needs to be edited to make it look like the following:

.. code-block:: toml

   [[outputs.nats]]
       ## URLs of NATS servers
       servers = ["nats://<nats_master_fqdn>:4222"]

       ## NATS subject for producer messages
       subject = "telegraf.metrics"

       ## Use Transport Layer Security
       secure = true

       tls_ca = "<telegraf_certs_directory>/root-ca.crt"
       tls_cert = "<telegraf_certs_directory>/telegraf_wo.crt.pem"
       tls_key = "<telegraf_certs_directory>/private/telegraf_wo.key.pem"

       data_format = "influx"

.. note:: In case of an agent remember to copy the certificates from the master to the agent machine.

.. rubric:: Configuration in Multitenant environments

|ne| provides a NATS user and its certificates also for each Tenant in multitenant environments.
By using a specific Tenant's user to send the metrics to the NATS master instance,
the complete separation of the data between Tenants is ensured.
The following configuration example shows how to use the specific ACME Tenant's NATS user.

.. note:: The certificate files are located in |ne| in `/root/security/nats-client/<tenant-name>/certs/`
          and in `/root/security/ca/root-ca.crt`. They need to be copied to the host where the Telegraf runs.

.. code-block:: toml

   [[outputs.nats]]
       ## URLs of NATS servers
       servers = ["nats://<nats_master_fqdn>:4222"]

       ## NATS subject for producer messages
       subject = "telegraf.metrics"

       ## Use Transport Layer Security
       secure = true

       tls_ca = "<telegraf_certs_directory>/root-ca.crt"
       tls_cert = "<telegraf_certs_directory>/acme-telegraf_wo.crt.pem"
       tls_key = "<telegraf_certs_directory>/private/acme-telegraf_wo.key.pem"

       data_format = "influx"


.. _write-data-to-nats-through-satellite:

Send metrics through Satellite
``````````````````````````````

.. note:: The Satellite must be reachable by Telegraf using the `Satellite FQDN`

In order to connect external Telegraf instances to the NATS master through
a Satellite, |ne| provides a set of certificates. These certificates are located in

.. code-block::

   /neteye/local/telegraf/conf/certs/telegraf-agent.crt.pem
   /neteye/local/telegraf/conf/certs/root-ca.crt
   /neteye/local/telegraf/conf/certs/private/telegraf-agent.key.pem

and must be copied to the machine you want to configure Telegraf on. Configure
file ownership and/or permissions in order to make the certificates and the key readable
by Telegraf.

Edit the output section of the configuration file of the Telegraf agent to make it look
like the following:

.. code-block:: toml

   [[outputs.nats]]
       ## URLs of NATS servers
       servers = ["nats://<satellite_fqdn>:4222"]

       ## NATS subject for producer messages
       subject = "telegraf.metrics"

       ## Use Transport Layer Security
       secure = true

       ## Optional TLS Config
       tls_ca = "<telegraf_certs_directory>/root-ca.crt"
       tls_cert = "<telegraf_certs_directory>/telegraf-agent.crt.pem"
       tls_key = "<telegraf_certs_directory>/private/telegraf-agent.key.pem"

       data_format = "influx"


.. warning:: Change configuration accordingly with your actual paths and Satellite FQDN.
   It is mandatory, however, that the subject matches **telegraf.metrics**, you can
   experience data losses otherwise.
