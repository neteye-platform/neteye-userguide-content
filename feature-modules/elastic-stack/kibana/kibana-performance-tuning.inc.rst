Kibana Performance
~~~~~~~~~~~~~~~~~~

There are a number of interesting tuning options that could be applied on Kibana
settings to improve performance on production.

For more information, see the `official
documentation <https://www.elastic.co/guide/en/kibana/8.18/production.html>`__.

.. rubric:: Require Content Security Policy (CSP)

Kibana uses a Content Security Policy to help prevent the browser from
allowing unsafe scripting, but older browsers will silently ignore this
policy. If your organization does not need to support Internet Explorer
11 or much older versions of our other supported browsers, we recommend
that you *enable Kibana’s strict mode* for content security policy,
which will block access to Kibana for any browser that does not enforce
even a rudimentary set of CSP protections.

To do this, set ``csp.strict`` to **true** in the file
``/neteye/shared/kibana/conf/kibana.yml``.

.. rubric:: Memory

Kibana has a default maximum memory limit of **1.4 GB**, and in most
cases, we recommend leaving this setting to its default value. However,
in some scenarios, such as large reporting jobs, it may make sense to
tweak limits to meet more specific requirements.

You can modify this limit by setting ``--max-old-space-size`` in the
``NODE_OPTIONS`` environment variable. In |ne| this can be configured
creating a file
``/etc/systemd/system/kibana-logmanager.service.d/memory.conf``
containing a limit in MB such as::

    [Service]
    NODE_OPTIONS="--max-old-space-size=2048"

For more information, see the `official
documentation <https://www.elastic.co/guide/en/kibana/8.18/production.html#memory>`__.

.. _kibana-user-customization:

.. rubric:: User Customization

The Kibana environment file :file:`/neteye/local/kibana/conf/sysconfig/kibana`
contains some options used by the Kibana service. Please note how
this file **must not be modified**, since it will be overwritten at each update.

The dedicated file :file:`/neteye/local/kibana/conf/sysconfig/kibana-user-customization`
can be used to specify or override one or more Kibana environment variables.
