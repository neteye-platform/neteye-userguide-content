
Shutdown Manager User
~~~~~~~~~~~~~~~~~~~~~

Shutdown manager uses a specific user called **shutdownmanager** to access
icinga2 APIs to initiate the shutdown command. The configuration is
automatically generated during ``neteye install`` with a random password and
must not be modified by the user.

Credentials can be found in the file
``/neteye/shared/icinga2/conf/icinga2/conf.d/shutdownmanager-users.conf``
e.g.::

    object ApiUser "shutdownmanager" {
      password = "c0tY...I1iZ"
      permissions = [ "objects/query/host","objects/query/service","actions/execute-command","objects/query/endpoint","objects/query/checkcommand"]
    }

**shutdownmanager** user has only permissions to query hosts, services, endpoints,
checkcommands and execute commands.
