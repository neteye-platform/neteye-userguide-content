.. _master-satellite-arch:

Master-Satellite Architecture
-----------------------------

The Master can communicate with clients directly, or through a
*Satellite* node. A Satellite forwards configurations from the Master to
a client, collects data with agents and sends it back to the Master.

The Master can manage multiple tenants. Each Satellite connected to
the Master is responsible for a set of hosts and/or services belonging
to one tenant. There may be several Satellites functioning on a single
tenant.

The monitoring configurations sent by the Master are individual for
each Satellite and clients in its zone (where applicable).
Communication between the Master and Satellites is secure and
encrypted.

.. _arch-satellite-communication:

Communication through a Satellite
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A **Satellite** is a NetEye instance which depends on a main NetEye
installation, the **Master**, and is responsible for different tasks,
including but not limited to,

- execute :ref:`Icinga 2 <active-monitoring>` checks and forward results to the Master
- collect logs and forward them to the Master
- collect assets with GLPI Agents
- forward data through :ref:`NATS <data-gathering-in-neteye-satellites>`
- collect data through :ref:`Tornado Collectors <tornado-collectors-overview>` and forward them
  to the Master to be processed by Tornado

NetEye implements secure communication between Satellites and Master;
each Satellite is responsible to handle a set of hosts.  On hosts can
be also installed different `agents`, software responsible to perform
different tasks on the host itself and are connected to the Satellite.

.. seealso:: Icinga 2 Agents are presented in section
   :ref:`monitor-conf-agent`

A Satellite proves useful in two scenarios: to offload the Master and
to implement multi tenancy.

As an example of the first scenario, consider an infrastructure that
needs to monitor a large number of servers and devices, possibly
located in multiple remote locations.

NetEye Satellites allow to reduce the load on Master and also the
number of requests between Master and hosts. Indeed, all the checks
are scheduled and executed by the Satellite and only their results are
forwarded to the Master.

The second scenario sees NetEye Satellites operate in an isolated
environment by implementing **multi tenancy**. For each tenant,
multiple satellites can be specified that are responsible for
monitoring and collecting logs. The Master receives data via
Satellites or directly from the agents and identifies each tenant
by means of the certificate installed on each Satellite.

The :command:`neteye satellites` commands - provided by |ne| out
of the box - can be very helpful to execute many operations to
configure and manage the |ne| satellites.

.. seealso:: Please refer to :ref:`neteye-satellite-configuration` to configure a Satellite;
   Update and upgrade procedures are explained in :ref:`neteye-update-satellites`,
   :ref:`neteye-upgrade-satellites-single` and :ref:`neteye-upgrade-satellites-cluster`,
   respectively.


Satellites use the NATS Server, the default message broker in NetEye, to transport
metrics and events (for example from Telegraf and Tornado collectors) between Satellites
and the Master.
Other components, such as Icinga 2 and GLPI, communicate with the Master and tenant servers
over secure HTTPS connections rather than via NATS.

If you want to learn more about NATS you can refer to the official `NATS documentation
<https://docs.nats.io/nats-concepts/intro>`__


.. _neteye-satellite-services:

Satellite Services
~~~~~~~~~~~~~~~~~~

The services that need to run on the NetEye Satellite Nodes are managed by a dedicated `systemd target`
named ``neteye-satellite.target``, which takes care of starting and stopping the services
of the Satellite when needed.

Various services are configured and activated out of the box on NetEye Satellites:
among others, the Tornado Collectors and those provided by Icinga 2.

For the complete list of the services enabled on a NetEye Satellite, on the Satellite you can execute:

.. code:: bash

   systemctl list-dependencies neteye-satellite.target
