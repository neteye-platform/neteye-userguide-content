
Once all Commands, Command Groups and Command Parameters have been
configured (and the **Director deploy** has been triggered), it is then
possible to launch the execution of the defined Commands.

GUI command execution
~~~~~~~~~~~~~~~~~~~~~

The execution of the Commands can be performed by authorized users from
the NetEye GUI. Under the menu entry **CMDO**, you will find the tabs
*Command Group*, *Command* and *Last Execution Result*.

In the *Command Group* tab you find the list of all the Command Groups
present in the Command Orchestrator. Clicking on one Command Group lets
you see the list children Commands of that Group. From the Commands
list, selecting one Command lets you configure the execution of the
Command, and execute it.

The *Command* tab shows the list of all the Commands defined in the
Command Orchestrator.

In the *Last Execution Result* tab you can find instead the result of
the last Command that you executed.

.. note:: The Last Execution Result will be available only until the Icinga2
   execution TTL expires (this is set in
   ``/neteye/shared/icingaweb2/conf/modules/cmdorchestrator/config.ini``
   and defaults to 20 minutes).

   The Last Execution Result tab only works within the web session in
   which you executed the Command. This means that after a logout from
   NetEye, the Execution Result will not be available anymore.

CLI Command Execution
~~~~~~~~~~~~~~~~~~~~~

From CLI it is possible to execute the Commands via REST API.

The endpoint for the command execution is
``https://<neteye_host>/neteye/cmdorchestrator/executecommand`` and the
request must be authenticated with the user's NetEye credentials.

The JSON payload of the request must contain:

* **command_id**: the ID of the Command that we want to execute
* **host**: the hostname of the host on which the Command must be
  executed. This field is only required if the command type of the
  associated *command_id* is *remote*.
* **parameter_values**: a (possibly empty) map ``"$parameter_name$":
  "parameter_value"`` that defines the value of the Command
  Parameters. The parameter value must be compliant with what was
  configured in the Command Parameter objects related to the Command.

Below you can find an example of a valid ``curl`` call to the
``executecommand`` REST API::

    curl -u '<neteye_username>:<neteye_password>' -XPOST \
            -H 'Accept: application/json' \
            -H "Content-Type: application/json" \
            'https://<neteye_host>/neteye/cmdorchestrator/executecommand'
            -d '{
                  "command_id": 1,
                  "host": "myhost",
                  "parameter_values": {
                    "$path_to_file$": "/tmp/myfile"
                  }
                }'

In case of the command execution is accepted, the response of the REST
API will be similar to the following::

    "{\"results\":[{\"checkable\":\"9b6ce3354241\",\"code\":202.0,\"execution\":\"793ad112-1edc-46bd-86de-7f885421a199\",\"status\":\"Accepted\"}]}"

The ``executecommand`` API aforementioned does not return the result of
the command because it will be executed asynchronously. To monitor the
command execution you can use the
``icingacli cmdorchestrator execution`` command to retrieve the
execution status.

* The **-\-token** parameter is mandatory, token value is returned in
  the response of ``executecommand`` REST API

Below you can find an example of a valid CLI command with the token
provided by the example above::

    icingacli cmdorchestrator execution show --token 793ad112-1edc-46bd-86de-7f885421a199
