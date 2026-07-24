.. _glpi-plugins:

Plugins
=======

To extend the functionality of GLPI, you can install additional plugins.
In NetEye, GLPI runs inside a container named ``systemd-glpi``.

Installing Plugins
------------------

To add a plugin to GLPI, you must place the plugin files in the plugins directory on the host system.
This directory is mounted into the container via a volume:

.. code-block:: text

   Volume=/usr/share/glpi/plugins:/var/www/glpi/plugins

You should copy your plugin files into ``/usr/share/glpi/plugins/`` on the host.

.. note:: In a cluster environment, the plugin files must be manually installed on **every node** of the cluster.

Enabling Plugins
----------------

After the plugin files are in place, you may need to run specific GLPI commands to enable or configure them.
You can access the GLPI container using the following command:

.. code-block:: bash

   neteye# podman exec -it systemd-glpi bash

Once inside the container, you can follow the official GLPI documentation for plugin enablement, which often involves running commands via the GLPI command-line interface.
