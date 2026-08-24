Icinga2 v2.16 migration tasks
=============================

This section explains how to migrate Icinga2 features ``ElasticsearchWriter`` and ``ElasticsearchDatastreamWriter`` that have been deprecated in v2.16 and
will be removed in future releases. Moreover, it explains how to migrate to the new ``FilterExpression`` permission
that will also be enforced in future releases.

These procedure should be executed after the |ne| 4.50 upgrade, to ensure a smooth transition to future
NetEye versions.


FilterExpression Permission
---------------------------

Icinga 2 v2.16 introduces the ``FilterExpression`` permission, which controls whether an
``ApiUser`` is allowed to use DSL filter expressions in API requests. This permission is
required for future Icinga 2 releases and should be prepared in advance.

In |ne| 4.50, existing API users can still use DSL filters, since the permission is not yet
fully enforced. Starting with the next upstream Icinga 2 release (v2.17), however, it will be enforced
and access will be denied unless the permission is explicitly granted.

To prepare your environment, review all Icinga2 ``ApiUser`` entries and enable ``FilterExpression`` for
those users that need to query the API with DSL filters.

NetEye also provides a service check named
``neteye-local!icinga2-filter-expression-permission-configured-neteyelocal``, which enters a
warning state when an ``ApiUser`` has used DSL filters in the last 24 hours without the
``FilterExpression`` permission enabled. This allows you to identify affected users before the
permission becomes mandatory.

For more information you can refer to the `official Icinga 2 documentation <https://icinga.com/docs/icinga-2/snapshot/doc/16-upgrading-icinga-2/#new-filter-expression-permission>`_.


Deprecation of ElasticsearchWriter and Elasticsearch Datastream Writer
----------------------------------------------------------------------

Starting with Icinga 2 v2.16, the legacy ``ElasticsearchWriter`` and
``ElasticsearchDatastreamWriter`` Icinga2 features are deprecated and will be removed in a
future NetEye release. They are replaced by the upstream ``OTLPWriter`` model, which is exposed in
NetEye through the :ref:`icinga2-features-otlpmetricswriter` feature.

If your environment still uses either of these legacy writers, you should migrate to the
recommended OTLP-based solution to ensure compatibility with future NetEye releases.

To migrate, disable the deprecated Icinga 2 feature, enable the new OTLP-based writer as
explained in :ref:`icinga2-features-otlpmetricswriter`, and review any dashboards,
integrations, or alerting rules that still depend on the previous datastream naming and
structure. If needed, update them to use the new Elasticsearch datastreams produced by the
OTLP metrics workflow.
