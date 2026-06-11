.. _tornado-director-executor:

Director
~~~~~~~~

Tornado Actions for creating hosts and services are available under
the DIRECTOR Action type.

The following elements of an Action are to be specified for the Director Executor
to extract data from a Tornado Action and prepare it to be sent to the `Icinga Director REST
API <https://icinga.com/docs/director/latest/doc/70-REST-API/>`__:

1. An **action_name**: ``create_host``, ``create_service`` would create an object
   of type ``host`` or ``service`` in the Director respectively.
   See more in the `official Icinga 2 documentation <https://icinga.com/docs/icinga-director/latest/doc/60-CLI/#create-a-new-object>`__.
2. An **action_payload** (optional): The payload of the Director action.

      .. code:: json

         {
            "object_type": "object",
            "object_name": "my_host_name",
            "address": "127.0.0.1",
            "check_command": "hostalive",
            "vars": {
               "location": "Bolzano"
               }
         }

3. An **icinga2_live_creation** (optional): Boolean value, which
   determines whether to create the specified Icinga Object also in
   Icinga 2.
