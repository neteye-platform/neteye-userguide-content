
Service Resource Management
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To manage service resources, several scripts have been developed by
the NetEye team and are provided with every NetEye installation. These
scripts are wrappers of the PCS and DRBD APIs and their use is
showcased in section :ref:`cluster-add-resource`.  Examples of
commands that are useful for NetEye Cluster troubleshooting are
introduced in section :ref:`cluster-management-commands`.


.. _cluster-nodes-roles:

Cluster Nodes Roles
```````````````````

In a |ne| cluster environment, some of the distributed |ne| services that run in multiple nodes
can be configured to run only on specific nodes among the cluster. This functionality is useful to
balance the load of the cluster nodes and be able to assign specific services to specific nodes
depending on the needs of the customer.

To assign a specific role to a node or to modify the roles configuration of the cluster, it is necessary to edit
the role assignation in the cluster configuration file :file:`/etc/neteye-cluster`, adding or modifying the
"roles" section of a specific node:

.. code-block:: json

    {
        "Hostname" : "my-neteye-cluster.example.com",
        "Nodes" : [
            {
                "addr" : "192.168.1.1",
                "hostname" : "my-neteye-01",
                "hostname_ext" : "my-neteye-01.example.com",
                "roles": [
                   "mariadb"
                ],
                "id" : 1
            },
        ]
    }

The roles that can be assigned to a node can be found in
:file:`/usr/share/neteye/cluster/config_validators/roles.d/`.

After modifying the configuration file, it is necessary to sync the cluster configuration
to all the nodes in the cluster. This can be done by executing the following command:

.. code:: bash

   cluster# neteye config cluster sync

Finally, to apply the changes to the cluster services configuration,
it is necessary to execute the install procedure focused on the service you want to apply
the changes to:

.. code:: bash

   cluster# neteye install --restrict-services-to <service_name>

Where the ``<service_name>`` is the name of the service or a list of
services separated by commas, usually related to the roles assigned
to the nodes in the cluster that has been modified.

Please refer to :ref:`neteye-install-advanced` for more information about the :command:`neteye install` command.

.. _cluster-services-configuration:

Cluster Services Configuration
``````````````````````````````

When dealing with services in a |ne| cluster, it is possible to define and configure
|ne| specific parameters related to the architecture, such as the IP address of the
service or the volume group to use.
The configuration of these parameters can be done in the dedicated file dedicated to
the service configuration located in :file:`/etc/neteye-services.d/<module_name>`. The folder
contains a set of YAML files, each one named as the service it refers to.

Once the configuration of a specific service has been performed, it is necessary to
apply the changes to the cluster services configuration. This can be done by
executing the install procedure focused on the service you want to apply the changes to:

.. code:: bash

   neteye install --restrict-services-to <service_name>

Where the ``<service_name>`` is the name of the service you want to apply the changes to.

Please refer to :ref:`neteye-install-advanced` for more information about the :command:`neteye install` command.

.. _cluster-add-resource:

Adding a Service Resource to a Cluster
``````````````````````````````````````

Service resources can be added by modifying an existing template,
located under the :file:`/usr/share/neteye/cluster/templates/`
directory, then copying it to a suitable location, and finally using
it in a script.

For example, consider the :file:`Services-core-nats-server.conf.tpl`
template.

.. code-block:: json

   {
       "volume_group": "vg00",
       "ip_pre" : "192.168.1",
       "Services": [
           {
               "name": "nats-server",
               "ip_post": "48",
               "drbd_minor": 23,
               "drbd_port": 7810,
               "folder": "/neteye/shared/nats-server/",
               "collocation_resource": "cluster_ip",
               "size": "1024"
           }
       ]
   }

Copy it, then edit it.

.. code:: bash

   cluster# cd /usr/share/neteye/cluster/templates/
   cluster# cp Services-core-nats-server.conf.tpl  /tmp/Services-core-nats-server.conf
   cluster# vi /tmp/Services-core-nats-server.conf

.. hint:: You can copy the edited file to any other location, to be
   used for reference or in case you need to change settings at any
   point in the future.

In the file, make sure to change the following values to match your
infrastructure network:

* `ip_pre`: the :ref:`corporate network address
  <neteye-cluster-architecture>` of the node (i.e., the first three
  octets).
* `ip_post`: the IP address of the node (**only** the last octet)

Once done, make sure that the JSON file you saved is valid
syntactically, for example by using the :command:`jq` utility::

  cluster# jq . /tmp/Services-core-nats-server.conf

A valid file will be displayed on, but if there is some **syntactic**
mistake in the file, an explanatory message will provide a hint to fix
the problem. Some possible message is shown next.

.. container:: codeblock

   .. code::

      parse error: Expected separator between values at line 7, column 21

      parse error: Objects must consist of key:value pairs at line 12, column 10

.. note:: Even if multiple errors are present in the file, only one
   error message is shown at a time, so always run ``jq`` until you
   see the whole content of the file instead of error messages: this
   will prove the file contains valid JSON.

Finally, let the cluster pick up the changes and configuration.

.. code-block:: bash

   cluster# cd /usr/share/neteye/scripts/cluster
   cluster# ./cluster_service_setup.pl -c /tmp/Services-core-nats-server.conf


.. _cluster-management-commands:

Cluster Management Commands
```````````````````````````

The most important commands used for checking the status of a (NetEye)
Cluster and to troubleshoot problems are:

* :command:`drbdmon`, a small utility to monitoring the DRBD devices
  and connections in real-time

* :command:`drbdadm`, DRBD's primary administration tool

* :command:`pcs`, used to manage a cluster, verify its resources,
  constraints, fencing devices and much more

.. hint:: You can find more information about all their
   functionalities and sub-commands in their respective manual pages:
   :manpage:`drbdmon`, :manpage:`drbdadm`, and :manpage:`pcs`.

In the remainder, we show some typical use of these commands, starting
from the simplest one.

.. code:: bash

   cluster# drbdmon

As its name implies, this command monitors what is happening in DRBD
and shows in real time a lot of information about the DRDB
status. Within the interface, any resource highlighted in red is in a
degraded status and therefore requires some inspection and fix. Click
:kbd:`p` to show only problematic resources.

The next command is the Swiss army knife of DRBD and is used to carry
out all configurations, tuning, and management of a DRBD
infrastructure. The most important option of the :command:`drbdadm`
command is ``-d`` (long option: ``--dry-run``): the command is
executed and behaves exactly like without the option, but it makes
**no changes** to the system.  This option should always be used
before making any change to the configuration, to check for possible
problems and unexpected side effects.

The command itself has a lot of options and sub commands, extensively
described in the above-mentioned man page. Within a NetEye Cluster,
the most used sub command is perhaps

.. code:: bash

   cluster# drbdadm --dry-run adjust all

This command checks the content of the configuration file and
synchronises the configuration on all nodes. The given command only
shows what would happen, remove the ``--dry-run`` option to actually
run it and make changes.

The third command is the main tool to manage the corosync/pacemaker
stack of a cluster: :command:`pcs`. Like :manpage:`drbdadm` it has a
number of sub commands and option.

.. code:: bash

   cluster# pcs status

This command prints the current status of the NetEye Cluster, its
nodes, and its resources, and allows to check whether there are any
ongoing issues.

In the output, right above the `Full list of resources`, all the nodes
(if any) are shown there, along with their state--`Online`/`Offline`
and `Standby` being the most common.

The presence of `Offline` nodes, that is, nodes disconnected form the
cluster or even shut down, is usually a sign of an ongoing problem and
requires a quick reaction. Indeed, the only legitimate situation when
a node can be `Offline` is after a planned reboot (like i.e., a kernel
update or a hardware upgrade).

If in the list of resources there is any resource marked as
**Stopped**, below the list and right above the `Daemon status` appear
some log entries for each stopped service. While these logs should
suffice to give some hint about the reason for the resource being
stopped, it is possible to check the full status and log files using
the commands :command:`systemctl status <resource name>` and
:command:`journalctl -u <resource name>`.

Additional sub commands of :command:`pcs` are:


.. code:: bash

   cluster# pcs property list

This command returns some information about the cluster and is similar
to the following snippet:

.. container:: codeblock

   .. code::

      Cluster Properties:
      cluster-infrastructure: corosync
      cluster-name: NetEye
      dc-version: 1.1.23-1.el7_9.1-9acf116022
      have-watchdog: false
      last-lrm-refresh: 1648467995
      stonith-enabled: false
      Node Attributes:
      neteye02.neteyelocal: standby=on

The important points here are:

* ``stonith-enabled: false``. This should always be ``true``, a value
  of ``false``, like in the example, implies that the cluster
  :term:`fencing` has been enabled for the node. This should happen
  only during maintenance windows, otherwise an immediate inspection
  is required because it may result in a split-brain situation.
  It is important to remark that `Fencing` must always be configured
  on a cluster before starting any resource.

* ``neteye02.neteyelocal: standby=on``. The node is in `Standby`
  status, meaning it can not host any running services or resources,
  but will still vote in the quorum.

.. seealso:: Fencing is described in great details in NetEye's blog
   post `Configuring Fencing on Dell Servers
   <https://www.neteye-blog.com/2020/06/configuring-fencing-on-dell-servers/>`_.

.. code:: bash

   cluster# pcs constraint

Returns a list of all active constraints on the cluster.


.. code:: bash

   cluster# pcs resource show [cluster_ip]

This command shows all the configured resources; if the parameter
``cluster_ip`` is added, shows only the Cluster IP address.


.. seealso:: For more information, troubleshooting options, and
   debugging commands, you can refer to RedHat's `Reference
   Documentation
   <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/assembly_overview-of-high-availability-configuring-and-managing-high-availability-clusters>`_
   for Pacemaker and high-availability, in particular Chapters `3. The
   pcs CLI
   <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/assembly_pcs-operation-configuring-and-managing-high-availability-clusters>`_,
   `9.7 Displaying fencing devices
   <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/assembly_configuring-fencing-configuring-and-managing-high-availability-clusters#proc_displaying-configuring-fence-devices-configuring-fencing>`_,
   and `10.3. Displaying Configured Resources
   <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/assembly_configuring-cluster-resources-configuring-and-managing-high-availability-clusters#proc_configuring-resource-meta-options-configuring-cluster-resources>`_.
