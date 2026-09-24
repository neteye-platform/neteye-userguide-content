.. _glpi-plugins:

Plugins
=======

In |ne|, GLPI runs inside a container named ``systemd-glpi``.
To extend the functionality of GLPI, you can install additional plugins.
Several plugins are available by default in the GLPI container,
so you can install and enable them directly from the GLPI web interface.
The files of those specific plugins are placed in the plugins directory ``/usr/share/glpi/plugins``,
mounted into the container via a volume:

.. code-block:: text

   Volume=/usr/share/glpi/plugins:/var/www/glpi/plugins

Here is the list of the bundled plugins:

* cloudinventory
* datainjection
* fields

Installing Plugins
------------------

To add an additional plugin to GLPI, you must place the plugin files in the GLPI data directory on the host system.
This directory is mounted into the container via a volume:

.. code-block:: text

   Volume=/neteye/shared/glpi/data:/var/glpi:U

You should copy your plugin files to ``/neteye/shared/glpi/data/marketplace/`` on the host.
This directory takes precedence over the plugins directory. This means that if a plugin is present in both
``/usr/share/glpi/plugins`` and ``/neteye/shared/glpi/data/marketplace/``, the files in the latter directory are used.

.. note:: In a cluster environment, the plugin files must be manually installed on the node where GLPI is running.

Enabling Plugins
----------------

After the plugin files are in place, you may need to run specific GLPI commands to enable or configure them.
You can access the GLPI container using the following command:

.. code-block:: bash

   neteye# podman exec -it systemd-glpi bash

Once inside the container, you can follow the official GLPI documentation to enable plugins, which often involves running commands via the GLPI command-line interface.
