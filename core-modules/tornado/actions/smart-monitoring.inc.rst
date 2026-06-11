.. _tornado-smartmon-check-executor:

Smart Monitoring Check Result
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SMART_MONITORING_CHECK_RESULT Action type allows to set a specific check result for a monitored object,
also in the cases when the Icinga 2 object for which you want to carry out the Action does not exist.
Moreover, the Smart Monitoring Check Result Executor responsible for carrying out the Action also ensures
that no outdated ``process-check-result`` will overwrite newer check results already present in Icinga 2.

Note however, that the Icinga agent cannot be created *live* using Smart
Monitoring Executor because it always requires a defined *endpoint* in the
configuration which is not possible since the Icinga API doesn't support
live-creation of an *endpoint*.

To ensure that outdated check results are not processed, the Action
``process-check-result`` is carried out by the
:ref:`tornado-icinga-executor` with the parameters ``execution_start``
and ``execution_end`` inherited by the Action definition or set equal
to the value of the ``created_ms`` property of the originating Tornado
Event.  Section
:ref:`tornado-smartmon-check-executor-discarded-check-results`
explains how the Executor handles these cases.

The SMART_MONITORING_CHECK_RESULT action type should include the following elements in its payload:

1. A **check_result**: The basic data to build the Icinga 2
   ``process-check-result`` action payload. See more in the official `Icinga 2 documentation <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#process-check-result>`__.

      .. code:: json

         {
            "exit_status": "2",
            "plugin_output": "Output message"
         }

   The **check_result** should contain all mandatory parameters expected by
   the Icinga API except the following ones that are automatically filled
   by the Executor:

      -  ``host``
      -  ``service``
      -  ``type``

2. A **host**: The data to build the payload which will be sent to the
   Icinga 2 REST API for the `host creation <https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#host>`__.

      .. code:: json

         {
            "object_name": "myhost",
            "address": "127.0.0.1",
            "check_command": "hostalive",
            "vars": {
               "location": "Rome"
            }
         }

3. A **service**: The data to build the payload which will be sent to
   the Icinga 2 REST API for the `service creation <https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#service>`__ (optional)

      .. code:: json

         {
            "object_name": "myservice",
            "check_command": "ping"
         }


.. _tornado-smartmon-check-executor-discarded-check-results:

.. rubric:: Discarded Check Results

Some ``process-check-results`` may be discarded by Icinga 2 if more recent
check results already exist for the target object.
In this situation the Executor does not retry the Action, but
simply logs an error containing the tag ``DISCARDED_PROCESS_CHECK_RESULT``
in the configured Tornado Logger. Please check out how to activate debug logging
for Tornado in the Troubleshooting section.

The log message showing a discarded ``process-check-result`` will be similar
to the following excerpt, enclosed in an ``ActionExecutionError``:

.. code::

   SmartMonitoringExecutor - Process check result action failed with error ActionExecutionError {
     message: "Icinga2Executor - Icinga2 API returned an unrecoverable error. Response status: 500 Internal Server Error.
       Response body: {\"results\":[{\"code\":409.0,\"status\":\"Newer check result already present. Check result for 'my-host!my-service' was discarded.\"}]}",
     can_retry: false,
     code: None,
     data: {
       "payload":{"execution_end":1651054222.0,"execution_start":1651054222.0,"exit_status":0,"plugin_output":"Some process check result","service":"my-host!my-service","type":"Service"},
       "tags":["DISCARDED_PROCESS_CHECK_RESULT"],
       "url":"https://icinga2-master.neteyelocal:5665/v1/actions/process-check-result",
       "method":"POST"
     }
   }.
