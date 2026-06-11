.. _tornado-icinga-executor:

Icinga 2
~~~~~~~~

The ICINGA 2 Action type allows to define one the existing `Icinga 2
actions <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#actions>`__.

For the Icinga 2 Executor to extract data from a Tornado
Action and prepare it to be sent to the `Icinga 2
API <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api>`_, the following
parameters are to be specified in the Action's payload:

1. An **icinga2_action_name**: The `Icinga 2
action <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#actions>`__ to perform.

2. An **icinga2_action_payload** (optional): should contain all mandatory parameters
   expected by the specific Icinga 2 action.

   .. code:: json

      {
         "exit_status": "2",
         "filter": "host.name==\"${_variables.hostname}\"",
         "plugin_output": "${event.payload.plugin_output}",
         "type": "Host"
      }
