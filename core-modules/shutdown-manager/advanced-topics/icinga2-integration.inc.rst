Icinga2 Integration
~~~~~~~~~~~~~~~~~~~

The Shutdown Manager takes advantage of the existing Icinga2 trust
infrastructure. Commands executed by the Shutdown Manager can either run
directly on the master node (e.g., for shutting down VMs via the vSphere
API), or on agents (e.g., calling */usr/bin/halt* on physical machines).

.. note:: Shutdown Commands are executed with the same permissions
   as the ``icinga2`` daemon itself. This could mean that by default
   your Icinga2 Agent does not have enough permissions to shut down its
   own host.

The shutdown of a host is triggered by calling the dedicated API
endpoint "shutdown-host". It takes as parameters:

* A host with an agent, in the form of a monitoring filter
  (e.g. ``"filter": "host.name==\"myagent.example.com\""``)
* A ``shutdown_command`` with arguments to be performed on the destination
  machine

The ``shutdown_command`` parameter **MUST** contain the ordered list of
arguments that the command should execute. The first argument must be
the command itself. (e.g. ``["/usr/bin/systemctl", "poweroff", "-i"]``)

Full example:

.. code:: bash

    curl -k -u $USER:$PW \
        -H 'X-HTTP-Method-Override: POST' \
        -H 'Accept: application/json' \
        -X POST 'https://192.0.2.14:5665/v1/actions/shutdown-host' \
        -d '{
               "type": "Host",
               "filter": "host.name==\"myagent.example.com\"",
               "shutdown_command": ["/usr/bin/systemctl", "poweroff", "-i"]
            }'

Depending on the ``run-on-agent`` setting of the associated shutdown
command, it will either be executed directly on the master node, or
otherwise passed to the specified agent and subsequently executed there.
