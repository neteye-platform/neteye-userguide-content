.. _data-gathering-in-neteye-satellites:

Data Gathering in |ne| Satellites
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| Satellites are used to collect data and send them to the
Master, where they are stored and processed; for example they can
later be used to set up dashboards.

Communication between the Master and Satellites is primarily handled by
the **NATS service**, which transports most monitoring and event data.
Only a few components, such as Icinga 2 and GLPI‑related communication,
bypass NATS and use their own HTTPS/secure channels.


.. _arch-nats-communication:

NATS Communication
``````````````````
NATS services are provided by one or more NATS server processes
that are configured to interconnect with each other and provide a NATS
service infrastructure throughout the |ne| Master and Satellites.

Data collected from a client is passed to a Master NATS Server
through `NATS Leaf Nodes <https://docs.nats.io/running-a-nats-service/configuration/leafnodes>`_.
A Leaf Node authenticates and authorizes
clients using local policy, allowing a relative Satellite to
securely communicate with the Master.

NATS Leaf Node authenticates Satellites to the Master NATS Server,
which can then assign the data coming from a Satellite to a single
|ne| Tenant, based on the configuration.

The Satellite initiates communication with the Master, so it's important
to ensure the :ref:`requirements for a NATS Leaf Node <table-cluster-tcp-communication-req>`
are met for the data transfer to be carried out successfully.

.. _fig-satellite-communication:

..  figure:: /getting-started/architecture/img/satellite-architecture.jpg

    Communication through NATS

.. _nats_leaf_multi_tenancy:

Multi Tenancy and NATS Leaf
```````````````````````````

One interesting functionality provided by NATS Server is the support for
a secure, TLS-based, **multi tenancy**, that can be secured using
multiple *accounts*. As stated in `Multi Tenancy using
Accounts <https://docs.nats.io/nats-server/configuration/securing_nats/accounts>`__,
NATS Server supports creating self-contained, isolated communications from multiple clients
to a single server, that will then independently process all data streams.
This ability is exploited by |ne|, where the single server runs on
the |ne| Master and the clients are the |ne| Satellites.

On each Satellite a NATS Server, which runs as **NATS Leaf Node**, collects
data from the hosts that the Satellite is monitoring and forwards them
to the |ne| Master's NATS Server. Thanks to the configuration done by |ne|,
when NATS Leaf Nodes authenticate to the Master's NATS Server,
they are automatically associated with the respective NATS Account
(which represents a Tenant) so that each Tenant's data flow is isolated.

.. _figure-satellite-architecture:

..  figure:: /getting-started/architecture/img/nats-server-on-master.jpg

    NATS Server in a multi-tenant environment.

On the Master, one Telegraf local Consumer instance called ``telegraf-local@neteye_consumer_telegraf_metrics`` is
employed to consume all the contents from the subject ``*.telegraf.metrics``. If you are in a cluster environment, an
instance of Telegraf local Consumer is started on each node of the cluster, to exploit the NATS built-in load balancing
feature called `distributed queue`. For more information about this feature, see the `official NATS documentation
<https://docs.nats.io/nats-concepts/queue>`__. Being |ne| Multi-Tenant capable, data is then tagged with the tenant name
based on the first part of the NATS subject (i.e. `acme_tenant.telegraf.metrics` refers to the subject for the
`acme_tenant` tenant) and the tenant's InfluxDB database to write into, so that Telegraf can dynamically write metrics
into the correct database to guarantee complete isolation.

To learn more about Telegraf configuration please check
:ref:`Telegraf Configuration section <telegraf-configuration>`


.. _nats_multi_tenancy_configuration:

Multi Tenancy configuration explained
`````````````````````````````````````

The procedure to :ref:`configure a |ne| Satellite
<neteye-satellite-configuration>` automatically configures `NATS Accounts
<https://docs.nats.io/nats-server/configuration/securing_nats/accounts>`__
on the Master and `NATS Leaf Node
<https://docs.nats.io/nats-server/configuration/leafnodes>`__ on the Satellites.
In this section we will give some insight into the most relevant configurations
performed by the procedure.

The automatic procedure configures the following:

#. NATS Server

    #. On the NATS Server of the |ne| Master, for each |ne| Tenant a
       dedicated Account is created. For each satellite a user is created and associated
       to its Tenant account. This is done with the purpose to isolate
       the traffic of each Tenant. This way, the NATS subscribers on the |ne| Master
       will receive the messages coming from the Satellites and from the Master itself.
       NATS Subscribers on a |ne| Satellite will not be able to access the messages
       coming from the other |ne| Tenants.


    #. The stream subjects coming from the |ne| Satellites are prefixed with the Tenant
       unique identifier defined during
       the :ref:`|ne| Satellite configuration <neteye-satellite-configuration>`.
       This is done in order to let subscribers securely pinpoint the origin of the
       messages, by solely relying on the NATS subject.
       So, for example, if the NATS Leaf Node of |ne| Satellite `acmesatellite` belonging to
       `tenantA` publishes a message on subject `mysubject`, NATS subscribers on the |ne| Master will
       need to subscribe to the subject `tenantA.mysubject` in order to receive the message.


#. NATS Satellite:

    #. A server certificate for the Satellite NATS Leaf Node is generated with the
       Root CA of the |ne| Satellite. This must be trusted by the clients that
       need to connect to the |ne| Satellite NATS Leaf Node.

    #. A client certificate is generated with the Root CA of the |ne| Master.
       This is used by the NATS Leaf Nodes to authenticate to the |ne| Master NATS
       Server.

    #. The NATS Leaf Node is configured to talk to the NATS Server of the |ne| Master,
       using the FQDN defined during the
       :ref:`|ne| Satellite configuration <neteye-satellite-configuration>` and the port 7422.
