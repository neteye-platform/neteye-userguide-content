

In order to avoid excessive, useless network traffic generated when the
cluster reallocates shards across cluster nodes after you restart an
Elasticsearch instance, NetEye employs *systemd* post-start and pre-stop
scripts to automatically enable and disable shard allocation properly on
the current node whenever the Elasticsearch service is started or
stopped by *systemctl*.

.. note:: By starting a stopped Elasticsearch instance, shard allocation
   will be enabled globally for the entire cluster. So if you have more
   than one Elasticsearch instance down, shards will be reallocated in
   order to prevent data loss.

Therefore best practice is to:
 * Never keep an Elasticsearch instance stopped on purpose. Stop it
   only for maintenance reasons (e.g. for restarting the server) and
   start it up again as soon as possible.
 * Restart or stop/start one Elasticsearch node at a time. If
   something bad happens and multiple Elasticsearch nodes go down,
   then start them all up again together.

.. _elastic-cluster-elastic-only:

Elastic-only Nodes
~~~~~~~~~~~~~~~~~~

From Neteye 4.9 it is possible to install Elastic-only nodes in order to
improve elasticsearch performance by adding more resources and processing
abilities to the cluster.

For more information on Single Purpose nodes please check out :ref:`Cluster
Architecture <cluster-node-types>`

To create an Elastic-only node you have to create an entry of type
ElasticOnlyNodes in the file ``/etc/neteye-cluster`` as in the following
example. Syntax is the same used for standard Node ::

   { "ElasticOnlyNodes": [
                {
             "addr" : "192.168.1.3",
             "hostname" : "my-neteye-03",
             "hostname_ext" : "my-neteye-03.example.com"
          }
       ]
   }

.. _elastic-cluster-voting-only:

Voting-only Nodes
~~~~~~~~~~~~~~~~~

From Neteye 4.16 it is possible to install Voting-only nodes in order to
add a node with a single purpose - to provide quorum. If |ne| Elastic Stack module
is installed, this node also provides voting-only functionalities
to Elasticsearch cluster.

This functionality is achieved configuring the node as a voting-only
master-eligible node specifying the variable
``ES_NODE_ROLES="master, voting_only"`` in the sysconfig file
``/neteye/local/elasticsearch/conf/sysconfig/elasticsearch-voting-only``.

Voting-only node is defined in ``/etc/neteye-cluster`` as in the following example ::

    { "VotingOnlyNode": {
             "addr" : "192.168.1.3",
             "hostname" : "my-neteye-03",
             "hostname_ext" : "my-neteye-03.example.com",
             "id" : 3
          }
    }

Please note that VotingOnlyNode is a json object and not an array because you can have a single Voting-only
node in a NetEye cluster.


Design and Configuration
~~~~~~~~~~~~~~~~~~~~~~~~

With NetEye 4 we recommend that you use at least 3 nodes to form an
Elasticsearch cluster. If nevertheless you decide to setup a 2-node
cluster, we recommend to consult a Würth IT Italy NetEye Solution Architect
who can fully explain the risks in your specific environment and help you
develop strategies to mitigate potential risks.

Elasticsearch `coordination
subsystem <https://www.elastic.co/guide/en/elasticsearch/reference/7.17/modules-discovery-hosts-providers.html>`__
is in charge to choose which nodes can form a quorum (note that all
NetEye cluster nodes are master eligible by default). The :command:`neteye install` script will
properly set *seed_hosts* and *initial_master_nodes* according to Elasticsearch's
recommendations and no manual intervention is required.

``neteye install`` will set two options to configure `cluster
discovery
<https://www.elastic.co/guide/en/elasticsearch/reference/7.17/modules-discovery.html>`__:

.. code::

    discovery.seed_hosts: ["host1", "host2", "host3"]
    cluster.initial_master_nodes: ["node1"]

Please note that the value for *initial_master_nodes* will be set only
on the first installed node of the cluster (it is optional on other
nodes and if set it must be the same for all nodes in the cluster).
Option *seed_hosts* will be set on all cluster nodes, included Elastic
Only nodes, and will have the same value on all nodes.

Elasticsearch reverse proxy
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Starting with NetEye 4.13, NGINX has been added to NetEye. NGINX acts as
a reverse proxy, by exposing a single endpoint and acting as a
load-balancer, to distribute incoming requests across all nodes and, in
this case, to all Elasticsearch instances. This solution improves the
overall performance and reliability of the cluster.

The elasticsearch endpoint is reachable at URI
https\://elasticsearch.neteyelocal:9200/. Please note that this is
the same port used before so no additional change is required; old
certificates used for elastic are still valid with the new
configuration.

All services connected elastic stack services like Kibana, Logstash and
Filebeat have been updated in order to reflect this improvement and to
take advantages of the new load balancing feature.
