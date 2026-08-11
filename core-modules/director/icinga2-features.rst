Integration with Elasticsearch
------------------------------

Icinga comes with a list of pre-configured modules to get started. These include the basic features
for Icinga to function, like the `api`, `checker`, `mainlog` and `notification`. Furthermore we
also ship some activated features, in specific the `livestatus`, `influxdb` and `icingadb`.
As a special feature that will only be pre-configured, but not automatically enabled,
we have the `neteye_datastreamwriter`.


|ne| DatastreamWriter
+++++++++++++++++++++

The `neteye_datastreamwriter` feature comes pre-configured with the `elastic-stack` module. It is
an alternative data sink for the `influxdb` writer that sends all the data to Elasticsearch. The
pre-configured feature ships the permissions for Elasticsearch and configures the writer with the
appropriate credentials in the form of client certificates.

Once activated, the feature will write the performance data of all checks out to Elasticsearch
Datastreams. Each Datastream has a name in the format `metrics-icinga2.<check>-<neteye_tenant>`
which includes the component template `metrics-icinga2@custom` for further optimizations, such as
setting a lifecycle policy.

The writer also allows you to further customize the behavior, like adding custom tags and labels or
filtering events out before sending them to Elasticsearch. To add additional information, please
take a look at the config file. :file:`/neteye/shared/icinga2/conf/icinga2/features-available/neteye_datastreamwriter.conf`
Every available option that's open to you is documented there until the feature with the official
documentation is released to the general public by Icinga2

..
  ToDo: Link to the documentation of icinga2 once the feature has been released.

To enable the feature run the following commands on the node that owns icinga2-master:

.. code:: shell

  master# icinga2-master feature enable neteye_datastreamwriter
  master# systemctl restart icinga2-master


.. note::
  You might want to disable the influxdb feature if you have enabled the `neteye_datastreamwriter` feature,
  as you are having the performance data duplicated otherwise, wasting disk space.
