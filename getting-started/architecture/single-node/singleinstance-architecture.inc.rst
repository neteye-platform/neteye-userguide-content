.. _neteye-single-node-architecture:

Single Node
-----------

|ne| can be run on a *Single Node Architecture*, i.e. as a
self-contained server.  Single Node is the basic variation of |ne|
setup, and is aimed at small environments that require limited
resources. Apart from that, running |ne| on a Single Node architecture
is applicable in case your infrastructure does not require high
availability. To maintain high availability of a monitoring
environment, you should better consider going with :ref:`Cluster
<neteye-cluster-architecture>` setup.

To start monitoring on a Single Node it only requires to :ref:`install
<system-installation>` |ne|, run :ref:`initial configuration
<neteye-initial-conf>`, and begin the actual process: define
services, hosts, etc.

A variety of |ne| services may be run on a Single Node installation to
fulfill monitoring functionality.

Single Node is a basic setup type, however you can enhance your
monitoring environment to a |ne| Cluster and/or set up several
tenants to monitor simultaneously. To fulfil the latter,
your environment might use Satellite nodes as actors that
help communicate data to the Master.
Satellite Node is to be :ref:`set up
<neteye-initial-conf>` together with a Single Node, and
:ref:`then configured <neteye-satellite-conf>` individually.

.. _fig-single-node-tenants:

.. figure:: /getting-started/architecture/img/architecture-single-node.svg

   A |ne| Single Node with tenants.


|ne| services on a |ne| *Single Node* installation are managed by
``systemd``. A single *systemd target* is a special *systemd unit* and
is responsible for managing ``start`` and ``stop`` operations of |ne|
services.

There can be more than one *systemd unit* available within installation,
however only one may be enabled. To find out which |ne| systemd target
is currently active, use the following command.

.. code:: bash

   neteye# systemctl list-units "neteye*.target"
