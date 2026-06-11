To guarantee an efficient yet reliable upgrade of the nodes in the Elasticsearch cluster, NetEye adopts
a strategy that upgrades nodes in parallel when possible, in order to save time. To troubleshoot
potential issues during the upgrade, it is important to understand how the procedure works.

Parallel Upgrade of Elasticsearch Nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Step 1: Group Nodes by Role**

Nodes are organized into logical groups based on their roles:

1. **Master** Nodes Group

   * Nodes with the **master** role.

2. **Data** Nodes Group

   * Nodes with the **data** role (excluding master).

3. **Data Tier** Groups

   * Nodes with tier roles: **hot**, **warm**, **cold**, **frozen**.

   * If a node has multiple tier roles, a combined group is created.

     * Example: a node with **cold** and **frozen** roles is placed in a group named **cold+frozen**.
       All nodes with either **cold** or **frozen** roles are included in this group.

   * Each node belongs to only one group.

**Step 2: Upgrade Sequence**

We upgrade the groups in the following order to maintain cluster health:

1. **Data Tier** Groups

   * Nodes are upgraded in parallel, but only one node per group at a time.

     For example:
       * One node from **hot**
       * One node from **warm**
       * One node from **cold+frozen**

2. **Data** Nodes

   * Nodes are upgraded sequentially, one node at a time.

3. **Master** Nodes

   * Nodes are upgraded sequentially, one node at a time.

Waiting for Shard Relocation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Updating or upgrading Elasticsearch requires restarting the service to take effect. During this process,
the shards allocated on the node being restarted are temporarily unassigned until the node is back online.

To ensure that upgrading node ``X`` does not cause shards to become completely unavailable, the procedure by default
waits until there are no unassigned shards whose replica is allocated on node ``X`` before proceeding with its upgrade.

.. note:: If a shard has no replicas (i.e., it is a primary shard without any replicas), it will become unavailable during the upgrade of the node hosting it.

By default, each node waits up to one hour for shard relocation to complete before continuing with the upgrade.
If relocation is not completed within this time frame, the procedure fails with an error, allowing you to investigate the issue.

In installations with large volumes of data, relocation may take longer. In such cases, you may choose to increase the waiting time
or maybe skip the relocation check entirely. Refer to the following sections for instructions.

Customize maximum relocation waiting time
`````````````````````````````````````````

You can customize the maximum waiting time for shard relocation by specifying two parameters when launching the update or upgrade command:
the number of retries and the seconds between each retry.

For example, to set a maximum waiting time of two hours:

.. code:: bash

  neteye# (nohup neteye update --extra-vars '{"es_status_wait_retries":120,"es_status_wait_seconds_between_retries":60}' &) && tail --retry -f nohup.out

.. code:: bash

  neteye# (nohup neteye upgrade --extra-vars '{"es_status_wait_retries":120,"es_status_wait_seconds_between_retries":60}' &) && tail --retry -f nohup.out

Skipping relocation wait
````````````````````````

If shard availability during the upgrade is not required in your installation, you can skip the relocation wait
using the `skip_es_status_to_wait` parameter:

.. code:: bash

  neteye# (nohup neteye update --extra-vars '{"skip_es_status_to_wait":true}' &) && tail --retry -f nohup.out

.. code:: bash

  neteye# (nohup neteye upgrade --extra-vars '{"skip_es_status_to_wait":true}' &) && tail --retry -f nohup.out

Waiting for a particular cluster status
```````````````````````````````````````

If the default behavior of waiting for shard relocation is not suitable for your installation,
you can configure the procedure to wait for a specific cluster status before upgrading each node.

For example, to wait until the cluster reaches ``green`` status:

.. code:: bash

  neteye# (nohup neteye update --extra-vars '{"es_status_to_wait": "green"}' &) && tail --retry -f nohup.out

.. code:: bash

  neteye# (nohup neteye upgrade --extra-vars '{"es_status_to_wait": "green"}' &) && tail --retry -f nohup.out

.. note:: Supported values are: ``green`` and ``yellow``.
