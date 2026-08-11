
This guide summarises the relevant configuration optimisations that
allows to optimize Elastic Stack and boost the performance to optimise
the use on Netye 4. Applying these suggestions proves very useful and is
suggested, especially on larger Elastic deployments.

.. _elastic-jvm-optimization:

Elasticsearch JVM
~~~~~~~~~~~~~~~~~

In Elasticsearch, the default options for the JVM are specified in the
:file:`/neteye/local/elasticsearch/conf/jvm.options` file.
Please note how this file **must not be modified**, since it will be
overwritten at each update.

If you would like to specify or override some options, a new ``.options`` file should
be created in the :file:`/neteye/local/elasticsearch/conf/jvm.options.d/` folder,
containing the desired options, one per line. Please note
that the JVM processes the options files according to the lexicographic order.

For example, we can set the encoding used by Java for reading and saving files to UTF-8
by creating a :file:`/neteye/local/elasticsearch/conf/jvm.options.d/01_custom_jvm.options`
with the following content::

    -Dfile.encoding=UTF-8

For more information about the available JVM options and their syntax, please
refer to the `official documentation <https://www.elastic.co/guide/en/elasticsearch/reference/current/advanced-configuration.html>`__.

Elasticsearch Database
~~~~~~~~~~~~~~~~~~~~~~

.. rubric:: Swapping

Swapping is very bad for performance, for node stability, and should be
avoided at any costs, because it can cause garbage collections to last
for minutes instead of milliseconds, it can cause nodes to respond
slowly, or even to disconnect from the cluster. In a resilient
distributed system, it proves more effective to let the operating system
kill the node than allowing swapping.

Moreover, Elasticsearch performs poorly when the system is swapping the
memory to disk. Therefore, it is vitally important to the health of your
node that none of the JVM is ever swapped out to disk. The following
steps allow to achieve this goal.

1. **Configure swappiness**. Ensure that the sysctl value
   ``vm.swappiness`` is set to **1**. This reduces the kernel’s tendency
   to swap and should not lead to swapping under normal circumstances,
   while still allowing the whole system to swap in emergency
   conditions. Execute the following commands on each Elastic Node and
   made changes persistent::

     sysctl vm.swappiness=1
     echo "vm.swappiness=1" > /etc/sysctl.d/zzz-swappiness.conf
     sysctl -p

2. **Memory locking**. Another best practice on Elastic nodes is use
   ``mlockall`` option, to try to lock the process address space into
   RAM, preventing any Elasticsearch memory from being swapped out. Set the
   ``bootstrap.memory_lock`` setting to **true**, so Elasticsearch will
   lock the process address space into RAM, preventing any portion of
   memory used by Elasticsearch from being swapped out.

   1. Uncomment or add this line to the
      ``/neteye/local/elasticsearch/conf/elasticsearch.yml`` file::

	bootstrap.memory_lock: true

   2. Edit limit of system resources on Service section creating the new
      file
      ``/etc/systemd/system/elasticsearch.service.d/neteye-limits.conf``
      with the following content::

        [Service]
        LimitMEMLOCK=infinity

   3. Restart resources::

        systemctl daemon-reload
        systemctl restart elasticsearch

   4. After starting Elasticsearch, you can see whether this setting was
      applied successfully by checking the value of ``mlockall`` in the
      output from this request::

	sh /usr/share/neteye/elasticsearch/scripts/es_curl.sh -XGET 'https://elasticsearch.neteyelocal:9200/_nodes?filter_path=**.mlockall&pretty'``

.. rubric:: Increase file descriptor


Check if the amount of file descriptor suffices by using the command
``lsof -p <elastic-pid> | wc -l`` on each nodes. By default the setting
on |ne| is 65,535.

To increase the default value this create a file in
``/etc/systemd/system/elasticsearch.service.d/neteye-open-file-limit.conf``
with content such as::

    [Service]
    LimitNOFILE=100000

For more information, see the `official
documentation <https://www.elastic.co/guide/en/elasticsearch/reference/7.17/file-descriptors.html>`__

.. rubric:: DNS cache settings

By default, Elasticsearch runs with a security manager in place, which
implies that the JVM defaults to caching positive hostname resolutions
indefinitely and defaults to caching negative hostname resolutions for
ten seconds. Elasticsearch overrides this behavior with default values
to cache positive lookups for **60** seconds, and to cache negative
lookups for **10** seconds.

These values should be suitable for most environments, including
environments where DNS resolutions vary with time. If not, you can edit
the values ``es.networkaddress.cache.ttl`` and
``es.networkaddress.cache.negative.ttl`` in the JVM options drop-in folder
:file:`/neteye/local/elasticsearch/conf/jvm.options.d/`.


Prevent Data from growing
~~~~~~~~~~~~~~~~~~~~~~~~~
Data is growing very fast, consuming too much cpu, RAM and disk.
If a system is over 50% of cpu or disk, an action should be taken, which is indicated by the Icinga checks
that help to take the right decisions.

.. rubric:: Limit data retention

Index Lifecycle Management (ILM) is designed to manage data retention by automating the lifecycle of
indices in Elasticsearch. It allows you to define policies that automatically manage indices based
on their age and performance needs, including actions like rollover, shrinking, force merging, and deletion.
This helps optimize storage costs and enforce data retention policies.

Learn more about configuring a lifecycle policy with an appropriate retention in `official documentation <https://www.elastic.co/docs/manage-data/lifecycle/index-lifecycle-management/configure-lifecycle-policy>`__.

.. rubric:: Save disk space

1. You can use a Time series data stream (TSDS) to store metrics data more efficiently. Metrics data stored in a TSDS
   may use up to 70% less disk space than a regular data stream. The exact impact will vary per data set.
   Learn more about when to use a TSDS in the `official documentation <https://www.elastic.co/docs/manage-data/data-store/data-streams/time-series-data-stream-tsds>`__.

2. Use logsdb index mode. Logsdb index mode significantly reduces storage needs
   by using slightly more CPU during ingest.
   After enabling logsdb index mode for your data sources, you may need to adjust cluster sizing
   in response to the new CPU and storage needs.
   logsdb mode is very easy to implement. To do that, just add the following parameter into the index
   template:

   .. code::

        {
         "index": {
           "mode": "logsdb"
        }

   To learn more about how logsdb index mode optimizes CPU and storage usage, check the blog on `Elasticsearch newly specialized logsdb index mode <https://www.elastic.co/search-labs/blog/elasticsearch-logsdb-index-mode>`__.

CPU and Data Ingestion
~~~~~~~~~~~~~~~~~~~~~~

It might happen that events are stored late or not stored at all.

.. important::

   In the case when data ingestion via Elastic agents is performed by means of a network listener,
   and a user is facing performance problems, they should use TCP in order to be able to see existing
   performance problems, because UDP is throwing away packets by design (in this case).

To provide a better latency or better throughput, you should set `predefined values <https://www.elastic.co/guide/en/fleet/8.18/es-output-settings.html#es-output-settings-performance-tuning-settings>`__ in the output definition
of the agent policy. If setting predefined values as shown above is not enough, try more advanced settings.

In order to receive **fast enough**, change advanced `yaml` configuration in a new output definition to define batch size, more workers, etc.
Besides settings for `ssl.certificate_authorities`, apply the following settings on big machines:

bulk_max_size: 1600
worker: 8
queue.mem.events: 100000
queue.mem.flush.min_events: 5000
queue.mem.flush.timeout: 10
compression_level: 1
idle_connection_timeout: 15

Make sure you find the best values matching your particular needs.

.. note:: The setting "Performance tuning" just above the advanced
   `yaml` configuration within the output definition will be set to custom, if you add such values into
   the advanced `yaml` configuration.


Memory / RAM settings
~~~~~~~~~~~~~~~~~~~~~

.. rubric:: Keep Satellite accessible

In some environments, Elastic Agent integrations can unexpectedly consume excessive memory due to different reasons.
When this happens, the Linux Kernel may invoke the OoM (Out of Memory) killer of systemd, terminating the Elastic Agent
service and usually, disrupting data ingestion.

To avoid the Agent being terminated when exceeding available memory, follow the tips
described on `our blog <https://www.neteye-blog.com/2025/06/elastic-integration-with-big-memory-usage-keep-host-accessible/>`__.

.. rubric:: Choose your license

If you're using Elastic under the Enterprise licence, GB of RAM are the basis of licensing,
and not the number of nodes. Hence, you should consider the `type of license <https://www.elastic.co/subscriptions>`__ you're using,
and in case with Enterprise license make sure the memory is enough to be distributed properly
between the nodes depending on your infrastructure.
