
El Proxy receives streams of log files from Logstash, signs them
in real time into a blockchain, and forwards them to
Elasticsearch. For more information, please check section
:ref:`ebp-architecture`

To make the use of El Proxy easier, different default pipelines, called
`input_beats`, `auditbeat`, `filebeat`, `winlogbeat`, `elastic_agent` and `ebp_persistent` pipelines
have been provided. The `input_beats` pipeline redirects logs to `main` or to the
beat specific pipelines (`auditbeat`, `filebeat`, `winlogbeat`).
If El Proxy is enabled for a specific host and the log matches the conditions
specified in the beat specific pipelines, logs will also be redirected to
`ebp_persistent` pipeline.

.. note::

    If the logs to be signed by El Proxy are collected by Elastic Agent, make sure to follow the section
    :ref:`signing-elastic-agent-documents` in order to configure the Elastic Agent instance properly.

Check out :ref:`ebp-custom-logs` for more information on redirecting other types
of events to El Proxy.

.. figure:: /feature-modules/elastic-stack/el-proxy/img/logstash-elproxy-architecture.png
   :alt: NetEye Logstash El Proxy architecture

   NetEye Logstash El Proxy architecture

Specifically, the `ebp_persistent` pipeline enables disk persistency,
extracts client certificate details and redirects data to the El
Proxy.

Please note that you need to have enough space in :file:`/neteye/shared/logstash/data/` for disk persistency.
The `ebp_persistent` pipeline is configured with three parameters:

  * ``queue.type``: specify if the queue is in memory or disk-persisted. If set to ``persisted`` the queue will
    be disk-persisted.
  * ``path.queue``: the path where the events will be persisted, by
    default :file:`/neteye/shared/logstash/data/ebp`
  * ``queue.max_bytes``: the maximum amount of data the queue can write, by default ``512mb``. Exceeding this
    limit may lead to a loss of events.

You can check and adjust the parameters for the queue in the file
:file:`/neteye/shared/logstash/conf/pipelines.yml`.

The user can customize the `ebp_persistent` pipeline by adding custom Logstash filters in the form of `.filters`
files in the directory :file:`/neteye/shared/logstash/conf/conf.ebp.d`.
Please note that the user must neither add `.input` or `.output` files nor modify existing configuration files.

.. warning:: Enabling El Proxy via the variable
    ``EBP_ENABLED`` in the file :file:`/neteye/shared/logstash/conf/sysconfig/logstash-user-customization`
    is not anymore supported. Please enable it per host as described below.

El Proxy can be enabled **per host** via Icinga Director. A **Host Template**, called **logmanager-blockchain-host**
is made available for this purpose.

To enable El Proxy on a host (we'll call the host **ACME**), we strongly suggest
to first create a dedicated host template, which imports `logmanager-blockchain-host`.
Then, configure the host ACME to inherit from this host template.

You can refer to Sections :ref:`host_templates` and to :ref:`add-host` for further
information about how to manage host templates and hosts in the Icinga Director.

As soon as you use the new dedicated template, the following fields are shown in the
**Host Configuration Panel** under **Custom properties**:

    * ``Enable Logging``: specify if logging for the current host must be enabled.
      If set to ``Yes`` log collection is enabled.
    * ``Blockchain Enable``: it appears only if ``Enable Logging`` is enabled.
      Specify if the log signature must be enabled for the current host. If set to ``Yes``,
      El Proxy is enabled for the host.
    * ``Blockchain Filter``: it becomes available only if ``Blockchain Enable`` is enabled.
      This field allows to configure which type of logs should be signed by El Proxy. For example
      if for some host you select **Only Authentication Logs**, the logs of this host will be signed by
      El Proxy only if they have category "authentication".

    * ``Blockchain Retention``: it becomes available only if ``Blockchain Enable`` is enabled.
      The retention policy specified for the logs of the current host (default value is 2 years or 730 days).
      Refer to Section :ref:`el_proxy_templates_and_retentions` for further
      information on retention policies.

.. note:: ``Enable Logging`` works in combination with ``Blockchain Enable``, both properties must
    be set to ``Yes`` to fully enable the log collection **and** the log signing.

Log and Blockchain properties of a hosts are regularly checked by
the **logmanager-director-es-index-neteyelocal** service,
which automatically stores them in Elasticsearch where they can be queried
and accessed when needed.

For additional information about El Proxy refer to section
:ref:`ebp-architecture`.

By default, El Proxy uses the common name (CN) specified in the Beats certificate as the customer name.
If the CN is not available, El Proxy uses a default customer name as a fallback.
The user can configure a custom default customer name by setting the variable ``EBP_DEFAULT_CUSTOMER``
in the file ``/neteye/shared/logstash/conf/sysconfig/logstash-user-customization``,
as follows::

  EBP_DEFAULT_CUSTOMER="mycustomer"

If the variable ``EBP_DEFAULT_CUSTOMER`` is not set, El Proxy will use the value "neteye" as default customer.
You must restart Logstash for the changes to take effect.

For additional information about El Proxy refer to :ref:`ebp-architecture`
