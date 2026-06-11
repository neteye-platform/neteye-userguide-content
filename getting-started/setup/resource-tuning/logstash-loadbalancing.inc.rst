How to Enable Load Balancing For Logstash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning:: This functionality is in beta stage and may be subject to changes.
   Beta features may break during minor upgrades and their quality is
   not ensured by regression testing.

The load balancing feature for logstash exploits NGINX ability to act as
a reverse proxy and distribute incoming (logstash) connections among all
nodes in the cluster. In this way, logstash will no longer be a cluster
resource anymore, but a standalone service running on **each node** of
the cluster.

Note however, that if you enable this feature, you will lose the ability
to sign the log files. This happens because with this setup, logmanager
has access *only* to the log files that are present on file systems
mounted *on the node where it is running*.

Indeed, **rsyslog** can not take advantage of the load balancing
feature, therefore only the logs on the node on which logmanager is
running will be signed.

In the case of **Beat**, log files will be sent through the load
balancer and therefore they will **not** be signed.

This how-to will guide you in setting up load balancing for logstash. In
a nutshell, you need first to disable the logstash cluster resource,
then to modify or to add logstash and NGINX configurations, and finally
to keep the logstash configuration in sync on all nodes.

More in details, this are the steps:

#. Permanently disable the cluster resource for logstash: run
   ``pcs resource disable logstash``
#. Create a *local service* of logstash on each node in the cluster, by
   following these steps:

   #. The configuration files will be stored in
      ``/neteye/local/logstash/conf``, so copy them over from
      ``/neteye/shared/logstash/conf``

      #. Fix all the paths into the conf files::

	   find /neteye/local/logstash/conf -type f -exec sed -i 's/shared/local/g' "{}" \;

   #. Edit **both** the
      ``/neteye/shared/logstash/conf/sysconfig/logstash`` and
      ``/neteye/local/logstash/conf/sysconfig/logstash`` files and add
      to them the following lines::

	LS_SETTINGS_DIR="/neteye/local/logstash/conf/"   OPTIONS="--config.reload.automatic"

   #. Add the host directive in
      ``/neteye/local/logstash/conf/conf.d/0_i03_agent_beats.input``
      (use the cluster internal network IP): host => "192.168.xxx.xxx"

   #. Create a new logstash service (call it e.g.,
      **logstash-local.service**) with the following content::

	[Unit]
	Description=logstash local

	[Service]
	Type=simple
	User=logstash
	Group=logstash
	EnvironmentFile=-/etc/default/logstash
	EnvironmentFile=-/neteye/local/logstash/conf/sysconfig/logstash
	ExecStartPre=/usr/share/logstash/bin/generate-config.sh
	ExecStart=/usr/share/logstash/bin/logstash "--path.settings" "/neteye/local/logstash/conf/" $OPTIONS Restart=always
	WorkingDirectory=/
	Nice=19
	LimitNOFILE=16384

	[Install]
	WantedBy=multi-user.target

   #. Add the service into the neteye cluster local systemd
      targets. You can refer to the :ref:`Cluster Technology and
      Architecture <neteye-cluster-architecture>` chapter of the user
      guide for more information.
   #. Edit file **/etc/hosts** to point the host
      **logstash.neteyelocal** to the cluster IP.

#. Add the NGINX load-balancing configuration in a file called
   **logstash-loadbalanced.j2** The name is very important because it
   will be used by :command:`neteye install` to setup the correct
   mapping between the logstash service and NGINX. The file needs to
   have the following content. Please, **pay special attention** in
   copying the whole snippet **AS-IS**, especially the three-line of the
   ``for`` cycle, because it is essential in configuring NGINX on *all*
   the cluster nodes::

    upstream logstash\_ingest {
       {% for node in nodes %}
         server {{ hostvars[node].internal\_node\_addr }}:5044;
       {% endfor %}
    }

     server {
       listen logstash.neteyelocal:5044;
       proxy_pass logstash_ingest;
     }

#. Remember that the logstash standalone configuration **must be kept
   in sync on all nodes**, therefore the
   **/neteye/local/logstash/conf/** directory must have the same
   content on all nodes. To achieve this goal you can for example set
   up a cron job that uses rsync to maintain the synchronisation.

#. Run :command:`neteye install` only once on any cluster node.

#. Start the local logstash service on every node:
   ``systemctl start logstash-local`` on every node
