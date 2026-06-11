.. _neteye-cluster-architecture:

Cluster
-------

.. _Pacemaker:
   https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/s1-pacemakeroverview-haao

The clustering service of NetEye 4 is based on the RedHat 8 High
Availability Clustering technologies, including Corosync, Pacemaker,
and DRBD, used to set up an :abbr:`HA (High Availability)` cluster
composed of a combination of operating nodes, Elastic-only nodes, and
Voting-only nodes. NetEye cluster is a **failover** cluster at service
level, meaning that it provides redundancy to avoid any downtime or
service disruption whenever one node in the cluster goes offline. In
such a case, indeed, services are moved to another node if necessary.

Reasons for a node to be offline include--but are not limited to:

* A networking issue (failure of a network interface or in the
  connectivity) which prevents a node to communicate with the other
  nodes
* A hardware or software issue which freezes or blocks a node
* A problem with the synchronisation of the data

All the cluster services run on a `dedicated network` called
**Corporate Network**: every cluster node has therefore **two** IP
addresses: A public one, accessible by the running service (including
e.g., SSH), and a private one, used by Corosync, Pacemaker, DRBD, and
Elastic-only nodes.

The figure :numref:`figure-neteye-cluster` illustrates the structure of a NetEye cluster and how it achieves High Availability.

**PCS-managed services** (e.g., Service A, Service B, ...) are typically composed of the following elements:

- A floating IP within the Corporate Network
- A DRBD device and a filesystem
- A (systemd) service

These services are dynamically allocated to operative nodes as needed.
For a complete list of PCS-managed services, use the command :command:`pcs status`.

On the other hand, **distributed replicated services** (e.g., Service 1, Service 2, ...) manage their own clustering
requirements and operate across multiple cluster nodes.
Details about distributed replicated services can be found in the :ref:`clustering-and-single-purpose-nodes` section.


.. _figure-neteye-cluster:

.. figure:: /getting-started/architecture/img/cluster-architecture.jpg
   :alt: NetEye cluster architecture.

   The NetEye cluster architecture.

If you have not yet installed clustering services, please turn to the
:ref:`Cluster Installation page <neteye-cluster-installation>` for
setup instructions.

.. _cluster-node-types:

Type of Nodes
~~~~~~~~~~~~~

Within a NetEye cluster, different types of nodes can be setup. We
distinguish between `Operative` and `Single Purpose` nodes, the latter
being either Elastic-only or Voting-only nodes. They are

Operative node
  On an operative node runs any services offered by NetEye, like e.g.,
  Tornado, Icinga 2, slmd, and so on. They can be seen as single nodes,
  connected by the clustering technologies mentioned above.

Elastic-only node
  Elastic-only nodes host **only** the DB component of the Elastic
  Stack, while FileBeat, Kibana, and other Elastic Stack components
  are still clusterised resources and run on operative
  nodes. Elastic-only nodes are used for either data storage or to add
  to the cluster more resources and processing abilities of
  elasticsearch data. In the latter case, the following are typical
  use cases:

  * Process log data in some way, for example with Machine Learning
    tools
  * Implement an `hot-warm-cold architecture
    <https://www.elastic.co/blog/implementing-hot-warm-cold-in-elasticsearch-with-index-lifecycle-management>`_
  * Increase data retention, redundancy, or storage to archive old
    data

.. note:: An operative node may also run services of the Elastic
   Stack, including its DB component. In other words, it is not
   necessary to have a dedicated node for Elastic services.

Voting-only node
  Nodes of this type are a kind of *silent* nodes: They do not run
  any service and therefore require limited computational resources
  compared to the other nodes. They are needed only in case of a node
  failure to establish the quorum and avoid cluster disruption.

.. topic:: Cluster Failure and Voting-only Nodes

   A cluster composed by *N* nodes requires that at least *N/2 + 1*
   **operative** nodes be online to operate properly (the
   `quorum`). For example, a cluster composed by 3 nodes needs in
   theory at least 2.5 nodes to operate properly. This means that
   whenever one of the three nodes goes offline, the cluster does not
   work anymore, because the quorum can not be reached. To make sure
   the cluster remains operational in these cases, adding a
   Voting-only node is the solution, because as soon as a node is
   offline, it will count as a regular node.

.. seealso:: Voting-only nodes and their use are described with great
   details in a NetEye blog post:
   https://www.neteye-blog.com/2020/03/neteye-voting-only-node/

The nodes of a cluster are listed in the :file:`/etc/neteye-cluster` file,
as in the example below:

.. code:: json

   {
     "Hostname" : "my-neteye-cluster.example.com",
     "Nodes" : [
        {
           "addr" : "192.168.1.1",
           "hostname" : "my-neteye-01",
           "hostname_ext" : "my-neteye-01.example.com",
           "id" : 1
        },
        {
           "addr" : "192.168.1.2",
           "hostname" : "my-neteye-02",
           "hostname_ext" : "my-neteye-02.example.com",
           "id" : 2
        },
        {
           "addr" : "192.168.1.3",
           "hostname" : "my-neteye-03",
           "hostname_ext" : "my-neteye-03.example.com",
           "id" : 3
        },
        {
           "addr" : "192.168.1.4",
           "hostname" : "my-neteye-04",
           "hostname_ext" : "my-neteye-04.example.com",
           "id" : 4
        }
     ],
     "ElasticOnlyNodes": [
        {
           "addr" : "192.168.1.5",
           "hostname" : "my-neteye-05",
           "hostname_ext" : "my-neteye-05.example.com",
           "id" : 5
        }
     ],
     "VotingOnlyNode" : {
          "addr" : "192.168.1.6",
          "hostname" : "my-neteye-06",
          "hostname_ext" : "my-neteye-06.example.com",
          "id" : 6
     },
     "InfluxDBOnlyNodes": [
         {
            "addr" : "192.168.1.7",
            "hostname" : "my-neteye-07",
            "hostname_ext" : "my-neteye-07.example.com"
         }
      ]
   }


.. _clustering-and-single-purpose-nodes:

Clustering and Single Purpose Nodes
```````````````````````````````````
The following services use their own native clustering capabilities
rather than Red Hat HA Clustering. NetEye will also take advantage of
their inbuilt load balancing capabilities.

Icinga 2 Cluster
   An Icinga 2 cluster is composed by one **master** instance holding
   configuration files and by a variable number of satellites and agents.

   .. seealso:: Icinga 2 clusters are described in great detail in the
      `official Icinga documentation
      <https://icinga.com/docs/icinga-2/latest/doc/06-distributed-monitoring/>`_

Elasticsearch
   Each cluster node runs a local master-eligible Elasticsearch
   service, connected to all other nodes. Elasticsearch itself chooses
   which nodes can form a quorum (note that all NetEye cluster nodes
   are master eligible by default), and so manual quorum setup is no
   longer required.

   .. seealso:: Elastic clusters and Elastic-only nodes  are described
      with more details in the :ref:`elastic-cluster` section.

Kibana
   The Kibana service is running on all the nodes that are
   configured to run it. Depending on the configuration in the
   :file:`/etc/neteye-cluster` file, Kibana can be run on multiple
   nodes, exploiting the native `Kibana load balancing capabilities
   <https://www.elastic.co/docs/deploy-manage/production-guidance/kibana-load-balance-traffic>`_.

   .. seealso:: The Kibana service is described in detail in the
      :ref:`kibana-architecture` section.

Galera
   The Galera cluster is a synchronous multi-master cluster for
   MariaDB. It is used to provide high availability and redundancy for
   the MariaDB database service. Each node in the Galera cluster can
   accept read and write requests, and changes made on one node are
   automatically replicated to all other nodes in the cluster.

   .. seealso:: Galera clusters are described in detail in the
      `official Galera documentation
      <https://galeracluster.com/library/documentation/index.html>`_.

   .. warning::

      When dealing with a Galera cluster, it is important to be aware
      of the following:

      - When restarting or starting a Galera node, the systemctl command will
        wait for the node to fully synchronize with the cluster before completing.
        This ensures that each node is properly aligned with the current cluster
        state and has consistent data before becoming operational. This
        synchronization process may take varying amounts of time depending on
        how much data needs to be transferred to bring the node up to date with
        the rest of the cluster.

      - The Galera cluster uses a quorum-based approach to ensure data consistency
        and availability. This means that a Galera Cluster will continue operating
        as long as more than half of the nodes (N/2 + 1) are up and synchronized.
        If the quorum is lost, the Galera cluster will block all operations to
        prevent data inconsistency across the cluster.

Node Roles
``````````

Among the different types of nodes in a cluster, it is possible to assign specific roles
to specific |ne| nodes, depending on the customer needs and the node capabilities.

For more information about the node roles, please refer to the :ref:`cluster-nodes-roles` section.


.. _clustering-services:

Clustering Services
~~~~~~~~~~~~~~~~~~~

The combination of the following software is at the core of the
NetEye's clustering functionalities:

* `Corosync <http://corosync.github.io/corosync/>`__: Provides group
  communication between a set of nodes, application restart upon
  failure, and a quorum system.
* Pacemaker_: Provides cluster management, lock management, and
  fencing.
* `DRBD <https://docs.linbit.com/docs/users-guide-9.0/>`__: Provides
  data redundancy by mirroring devices (hard drives, partitions,
  logical volumes, etc.) between hosts in real time.

“Local” NetEye services running *simultaneously* on *each* NetEye node
( i.e. *not* managed by Pacemaker and Corosync ), are managed by a
dedicated *systemd target* unit called
``neteye-cluster-local.target``.  This reduced set of local services
is managed exactly alike the :ref:`Single Node neteye target
<neteye-single-node-architecture>`::

   # systemctl list-dependencies neteye-cluster-local.target

.. container:: codeblock

   .. code::

      neteye-cluster-local.target
      ● └─drbd.service
      ● └─elasticsearch.service
      ● └─icinga2.service
      [...]

.. _cluster-management:

Cluster Management
~~~~~~~~~~~~~~~~~~

There are several CLI commands to be used in the management and
troubleshooting of clusters, most notably :command:`drbdmon`,
:command:`drbdadm`, and :command:`pcs`.

The first one, :command:`drbdmon` is used to monitor the status of
DRBD, i.e., to verify if the nodes of a cluster communicate flawlessly
or if there is some ongoing issue, like e.g., a node or network
failure, or a `split brain
<https://linbit.com/drbd-user-guide/drbd-guide-9_0-en/#s-split-brain-notification-and-recovery>`_.

The second command, :command:`drbdadm` allows to carry out
administrative tasks on DRBD.

Finally, the :command:`pcs` command is used to manage resources on a
`pcs cluster` only; its main purpose is to move services between the
cluster nodes when required.

.. note:: In order to keep data and configuration synchronisation,
   `pcs clusters` rely on Corosync, Pacemaker, and DRBD.
   They are composed by operating nodes and Voting-only nodes, while Elasticsearch nodes are
   completely invisible to `pcs clusters`. Voting-only nodes show up
   in the cluster status only by using the command :command:`pcs
   quorum status`.

In particular, :command:`pcs status` retrieves the current status of
the nodes and services, while :command:`pcs node standby` and
:command:`pcs node unstandby` put a node offline and back online,
respectively.

More information and examples about these command can be found in
section :ref:`cluster-management-commands`.
