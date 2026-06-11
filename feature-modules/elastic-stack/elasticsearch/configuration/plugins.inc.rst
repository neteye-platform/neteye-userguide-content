
Plugins
~~~~~~~

Plugins extend the core functionality of Elasticsearch. They range
from adding custom mapping types, custom analyzers, native scripts,
custom discovery and more.

Plugins can come from different sources: the official ones created or at least maintained by Elastic,
community-sourced plugins from other users, and plugins that you provide.

Core plugins are part of Elasticsearch project, and are delivered at the same time as Elasticsearch.
Their version number always matches the version number of Elasticsearch itself.

Community contributed plugins are external to the Elasticsearch project. They are provided by individual developers or private companies
and have their own licenses as well as their own versioning system.

Plugins contain JAR files, but may also contain scripts and config files, and must be installed
on every node in the cluster.

Run the following command to source sysconfig variables:

.. code:: bash

    . /usr/share/neteye/elasticsearch/scripts/es_autosetup_functions.sh; source_elasticsearch_sysconfig

Now, an actual plugin command can be run:

.. code:: bash

    ES_PATH_CONF=${ES_PATH_CONF} /usr/share/elasticsearch/bin/elasticsearch-plugin install [plugin_name]


This command will install the version of the plugin that matches your Elasticsearch version.
Upon every `neteye update` / `neteye upgrade` the plugins will be updated to the latest
version available.

A plugin can also be downloaded directly from a custom location by specifying the URL,
from your local file system, or from an HTTP URL. Please consult
`official installation guide <https://www.elastic.co/docs/reference/elasticsearch/plugins/plugin-management-custom-url>`__
for more details on various plugin installation methods.

After installation, each node must be restarted before the plugin becomes visible.

Some of the official plugins are always provided with the service, and can be
`enabled per deployment <https://www.elastic.co/docs/reference/elasticsearch/plugins/plugin-management#enabling-plugins-for-a-deployment>`__.

.. note::
  When running `neteye update` / `neteye upgrade` for deployments with community contributed
  plugins installed, the latter must be manually removed from all nodes before the running the procedure,
  and re-installed after the procedure is successfully completed. This will prevent `neteye update` / `neteye upgrade`
  from failing due to not being able to automatically re-install a plugin from a custom source.

Check out the `official Elasticsearch guide <https://www.elastic.co/docs/reference/elasticsearch/plugins/listing-removing-updating>`__
to find more information on plugin management options.
