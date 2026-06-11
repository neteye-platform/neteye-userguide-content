.. _elastic_filebeat_netflow_configuration:

Filebeat Netflow module specific configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In NetEye, Filebeat is shipped with the `NetFlow
Module <https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-module-netflow.html>`__
included.
The module should be configured without directly modifying the configuration file. Any change in the
configuration file will be overwritten during future updates and result in the reactivation
of the module or losing particular customizations.

The correct configuration can be accomplished by setting the following three environment
variables in the ``/neteye/shared/filebeat/conf/sysconfig/filebeat-user-customization``
environment file:

-  **NETFLOW_ENABLED**: which can be used to enable or disable the listening of logs
   on the specified host and port. Default: *true*
-  **NETFLOW_HOST**: The host to which the module will listen on. Default: *0.0.0.0* (all hosts)
-  **NETFLOW_PORT**: The port to listen on. Default: *2055*

After changing the value of any of the aforementioned variables, the ``neteye install`` command should
be executed to apply the changes::

    neteye# neteye install

.. warning:: If the module is deactivated using the :command:`filebeat modules disable`
   command and not using the associated environment variable, Filebeat will rename the
   configuration file and, at the next update, the module will be re-installed
   and re-activated.
