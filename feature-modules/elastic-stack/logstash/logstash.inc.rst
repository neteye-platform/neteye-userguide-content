
Logstash on NetEye ships with an `Elastic Stack
template <https://www.elastic.co/guide/en/elasticsearch/reference/7.17/indices-templates.html>`__,
which allows to manage its configuration within the NetEye
environment.

Furthermore, please note how all pipelines configuration files, located in the :file:`/neteye/shared/logstash/conf/conf.*.d`
folders, are set as *config files*, which prevents them from being silently overwritten
by future updates. As mentioned also in the :ref:`.rpmsave and .rpmnew migration guide <rpm-migration>`,
*config files* will instead lead to an *rpmnew* file if they were modified both on the system
**and** by the update, enabling so the user to control their migration.

.. note::

   Since migrating ``.rpmnew`` and ``.rpmsave`` files for Logstash pipelines is not
   straightforward, it is recommended to define custom pipelines in a separate
   configuration file and include them via ``pipelines.yml``, rather than modifying
   or extending the default pipeline configuration files delivered with the product.

   As ``pipelines.yml`` is itself a configuration file, updates will generate an
   ``.rpmnew`` file instead of overwriting existing changes. This approach generally
   simplifies migration, as only the pipeline inclusion needs to be reviewed and
   merged.

   Example of including a custom pipeline in ``pipelines.yml``::

      - pipeline.id: custom_pipeline
        path.config: "/neteye/shared/logstash/conf/custom_pipeline.conf"



Logstash Index Template
```````````````````````

|ne| configures an index template `logs-logstash` dedicated to Logstash logs. Any log coming from the Logstash
main pipeline, that will mainly manage rsyslog logs and user-customized input sources, will match the
`logs-logstash-*` index template, which will create the dedicated data stream.

In order to modify the retention policy applied to such logs, you can set the desired retention period in the
data stream control panel, by selecting :menuselection:`Manage / Edit data retention`.

Autoexpand Replicas
```````````````````
Configuration of Logstash replica that applies to both single instances and clusters is done by means of the
"neteye-autoexpand-replicas" component template applied to the Logstash index template `logs-logstash`. The new indices
matching the pattern `logs-logstash-*` will automatically configure the
replica with the range 0-1 using the `index.auto_expand_replicas
<https://www.elastic.co/guide/en/elasticsearch/reference/7.17/index-modules.html#dynamic-index-settings>`__
setting.
