.. _kibana-architecture:

Architecture
~~~~~~~~~~~~

Kibana, as a powerful visualization tool for exploring and analyzing your data in real-time integrated into the
Elastic Stack, is used by |ne| for providing a user-friendly interface for interacting with Elasticsearch data.

In order to enhance the availability and scalability of Kibana, it is possible to run multiple instances of Kibana
across different nodes in the |ne| cluster. This multi-instance architecture allows for better load balancing and
improved performance, especially in larger deployments where Kibana is heavily utilized.

The multi-instance architecture is based on Nginx and Keepalived, which work together to provide a highly available
and scalable Kibana service. Nginx acts as a reverse proxy, distributing incoming requests to the available Kibana instances,
while Keepalived ensures that the service remains available even in the event of node failures by managing virtual IP addresses.

Kibana instances can be configured through the :file:`/etc/neteye-cluster` file, where you can specify which nodes
should host the Kibana service by adding the role "kibana" to the nodes' roles field.

.. note::

    For a valid configuration of the |ne| cluster, at least one node must be assigned the "kibana" role and
    the kibana role can be assigned to any node in the cluster (including elastic only nodes) except for the voting node.

Once the Kibana nodes are defined in the cluster configuration, launching the :command:`neteye install` command will automatically
initialize the Kibana instances on the specified nodes.

.. note::

    Refer to :ref:`cluster-nodes-roles` and :ref:`cluster-services-configuration` for more information on how to configure
    the Kibana services in the cluster nodes.
    When configuring a Kibana cluster instance, use the following configuration template file for setting up the chosen IP for Kibana::

        $ cat /etc/neteye-services.d/elastic-stack/kibana.yaml.tpl

    Check the template file and adapt only the settings present in it, i.e. only the cluster IP needs to be defined,
    and no other parameters added.
