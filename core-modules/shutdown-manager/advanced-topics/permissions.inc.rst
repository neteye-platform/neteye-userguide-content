Shutdown Host Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~

The shutdown of a given host using the configuration described in the
previous sections can also be initiated by calling the
*/v1/actions/shutdown-host* endpoint of an Icinga2 master or satellite
node.

If you want to add your own automation for shutting down hosts, you will
need to `configure a valid Icinga2 API
user <https://icinga.com/docs/icinga2/latest/doc/12-icinga2-api/#authentication>`__
and grant the *actions/shutdown-host* permissions as in this example:

::

    object ApiUser "shutdown-automation" {
      password = "secret"
      permission = "actions/shutdown-host"
    }

.. note:: Users with all permissions ("\*") will also be able to initiate
   shutdown procedures for eligible hosts!

You can then authenticate yourself to the Icinga2 API via BasicAuth.
