
.. _retrieve-payload:

Retrieving Payload of an Event
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before you start creating your Tornado configuration, the payload of each received event
is to be extracted. This can be done by following the next steps with the Processing Tree:

* Create a Filter which matches all incoming events of a chosen type (let's use SNMP Traps as an example):

.. code:: json

   {
     "type": "AND",
     "operators":
       [
         {
           "type": "equals",
           "first": "${event.type}",
           "second": "snmptrapd"
         }
       ]
   }

* Create a Rule for the 'snmptrap' filter by clicking on 'Add rule'.
  The **archive_all** rule would then write all incoming traps in JSON format to a log file,
  which can be defined in the :file:`/neteye/share/tornado/conf/archive_executor.toml`.

  If nothing is defined, all logs are written to the :file:`/neteye/shared/tornado/data/archive/all/one_events.log`.

* Define the following action within a Rule:

.. code:: json

    [
      {
        "id": "archive",
        "payload": {
          "archive_type": "snmptrapd",
          "event": "${event}",
          "hostname": "${event.payload.src_ip}"
        }
      }
    ]
