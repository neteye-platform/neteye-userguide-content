
.. _verify_module_running:

Verify if a module is running correctly
---------------------------------------

After installing a |ne| Component, you need to make sure that all
services are running.

The commands to be used differ on a :ref:`Single Node
<verify-neteye-single>` and on a :ref:`Cluster
<verify-neteye-cluster>` Installation.

.. _verify-neteye-single:

Verify Installation on |ne| Single Node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye status` command outputs a list of the status of
all |ne| services, similar to the following snippet:

.. container:: codeblock

   .. sourcecode::

      DOWN [3] elastic-blockchain-proxy.service
      DOWN [3] elasticsearch.service
      UP   [0] filebeat.service
      UP   [0] grafana-server.service
      UP   [0] httpd.service
      DOWN [3] icinga2-master.service
      UP   [0] influxdb.service
      DOWN [3] kibana-logmanager.service
      DOWN [0] lampod.service
      UP   [0] logstash.service
      UP   [0] mariadb.service
      DOWN [3] nats-server.service
      UP   [0] neteye-agent.service
      UP   [0] nginx.service
      UP   [0] nprobe.service
      UP   [0] ntopng.service
      UP   [0] redis.service
      UP   [0] rh-php73-php-fpm.service
      UP   [0] rsyslog-logmanager.service
      UP   [0] slmd.service
      UP   [0] smsd.service
      UP   [0] snmptrapd.service
      UP   [0] tornado.service
      DOWN [3] tornado_email_collector.service
      DOWN [0] tornado_icinga2_collector.service
      DOWN [3] tornado_nats_json_collector.service
      DOWN [3] tornado_webhook_collector.service

.. note:: Output may vary, depending on both installed modules and
   running services.

Suppose you have just install Tornado and all its collectors: they
should be running, but are marked as ``DOWN``. This means that
something has gone wrong and you need to understand why. You can
therefore check the dedicated :ref:`troubleshooting section
<ts-restart-services-single>` for directions.

.. _verify-neteye-cluster:

Verify Installation on |ne| Cluster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On a cluster it is necessary to differentiate between *clustered* and
*non clustered* services: *Non clustered services*, which for example
include Elasticsearch, follow the same approach shown in :ref:`the
previous section <verify-neteye-single>` and in case of issues, can be
inspected with the same commands mentioned in the corresponding
:ref:`troubleshooting section <ts-restart-services-single>`.

*Clustered services*, on the contrary, require a different
approach. Indeed, the :command:`neteye status`, :command:`neteye
start`, and :command:`neteye stop` commands can not be used, because
they are not available on cluster.

.. note:: *Clustered services* are referred to as **Resources**. For
   example, a Tornado instance running on a |ne| single installation
   is a **service**, while a Tornado instance running on a |ne|
   cluster is a **resource**.

Therefore, to verify if resources are correctly running, use the
:command:`pcs status` command, which outputs the status of the cluster
and all the resources, similarly to the following excerpt.

.. container:: codeblock

   .. sourcecode::

      Cluster name: NetEye
      Stack: corosync
      Current DC: neteye01.local (version 1.1.23-1.el7_9.1-9acf116022) - partition with quorum
      Last updated: Wed Jul 28 09:47:52 2021
      Last change: Tue Jul 27 15:04:36 2021 by root via cibadmin on neteye02.local
      2 nodes configured
      74 resource instances configured
      Online: [ neteye01.local neteye02.local ]
      Full list of resources:
       cluster_ip    (ocf::heartbeat:IPaddr2):    Started neteye02.local
       Resource Group: tornado_rsyslog_collector_group
           tornado_rsyslog_collector_drbd_fs    (ocf::heartbeat:Filesystem):    Started neteye02.local
       Resource Group: tornado_group

In case a resource is not starting correctly, it will be listed at the
end of the output (see snippet below) as **Failed**. You need to
understand why it is not running: the dedicated :ref:`cluster
troubleshooting section <ts-restart-services-cluster>` features
options that you can apply to find the root cause of the problem.

.. container:: codeblock

   .. sourcecode::

      Failed Resource Actions:
      * tornado_email_collector_monitor_30000 on neteye02.local 'not running' (7): call=414, status=complete, exitreason='',
          last-rc-change='Wed Jul 28 09:57:21 2021', queued=0ms, exec=0ms
