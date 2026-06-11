.. _logstash-plugins:

Plugins
```````
Logstash provides a variety of plugins, i.e. input, filter, codec, and output plugins, which
serve to enhance its functionality in a custom manner.

Some plugins are shipped with Logstash by default, however, there is a possibility to add
plugins on top of the ones that are available in your deployment.

All the plugins supported at various levels can be found in the `Elastic Support Matrix <https://www.elastic.co/support/matrix/#logstash_plugins>`__.

You can list the plugins currently available in your deployment by running the following subcommand:

.. code:: bash

   /usr/share/logstash/bin/logstash-plugin list

Check out the official Logstash guide for instructions on how to `install <https://www.elastic.co/docs/reference/logstash/working-with-plugins#installing-plugins>`__
and `remove <https://www.elastic.co/docs/reference/logstash/working-with-plugins#removing-plugins>`__ additional plugins.

On top of that, you can check the plugins that were installed additionally by running

.. code:: bash

   python3 /usr/share/neteye/logstash/scripts/configurator/logstash_plugin_manager.py additional-plugins-installed

Check out more listing options in the `Logstash official documentation <https://www.elastic.co/docs/reference/logstash/working-with-plugins#listing-plugins>`__.

Plugins have their own release cycles and are often released independently of Logstash's
core release cycle. With every NetEye update, Logstash is being updated together with the plugins
available in your deployment.

For the plugins that were installed additionally, on top of the ones that were shipped with Logstash
by default, you can use the update subcommand to get the latest version of the plugin:

.. code:: bash

   /usr/share/logstash/bin/logstash-plugin update [PLUGIN]

.. warning:: It is not recommended to update or remove the plugins shipped by default manually, since every
   `neteye update` / `neteye upgrade` will overwrite those changes and reinstall or update
   the plugins to the latest version available. In some cases it may also lead to Logstash not operating properly.

More information about Logstash plugins can be found in `Logstash official documentation <https://www.elastic.co/docs/reference/logstash/working-with-plugins>`__.
