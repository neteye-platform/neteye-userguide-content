.. _tornado-icinga-collector-exec:

Icinga 2
~~~~~~~~

The Icinga 2 Collector subscribes to the `Icinga 2 API event
streams <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#event-streams>`_,
generates Tornado Events from the Icinga 2 Events, and publishes them on
the Tornado Engine.

On startup, it connects to `Icinga 2 Server API
<https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/>`_ and
subscribes to user defined `Event Streams
<https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#event-streams>`_.


In order to configure the stream, create a JSON file in :file:`/neteye/shared/tornado_icinga2_collector/conf/streams/`.
More than one stream subscription can be defined.

For each stream, you must provide two values in order to successfully create a subscription:

-  *stream*: the stream configuration composed of:

   -  *types*: An array of `Icinga 2 Event
      types <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#event-stream-types>`_;
   -  *queue*: A unique queue name used by Icinga 2 to identify the
      stream;
   -  *filter*: An optional Event Stream filter. Additional information
      about the filter can be found in the `official
      documentation <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#event-stream-filter>`_.

-  *collector_config*: The transformation logic that converts an Icinga 2
   Event into a Tornado Event.

Below you may find examples of valid content for a stream configuration JSON file:

For all Icinga 2 events

.. code:: json

   {
     "stream": {
       "types": ["CheckResult",
                 "StateChange",
                 "Notification",
                 "AcknowledgementSet",
                 "AcknowledgementCleared",
                 "CommentAdded",
                 "CommentRemoved",
                 "DowntimeAdded",
                 "DowntimeRemoved",
                 "DowntimeStarted",
                 "DowntimeTriggered"],
       "queue": "icinga2_AllEvents_all"
     },
     "collector_config": {
       "event_type": "icinga2_AllEvents_all",
       "payload": {
         "response": "${@}"
       }
     }
   }

For check result events

.. code:: json

   {
     "stream": {
       "types": ["CheckResult"],
       "queue": "icinga2_CheckResult_all"
     },
     "collector_config": {
       "event_type": "icinga2_CheckResult_all",
       "payload": {
         "response": "${@}"
       }
     }
   }


For notification events

.. code:: json

   {
     "stream": {
       "types": ["Notification"],
       "queue": "icinga2_Notification_all"
     },
     "collector_config": {
       "event_type": "icinga2_Notification_all",
       "payload": {
         "response": "${@}"
       }
     }
   }

For statechange events

.. code:: json

   {
     "stream": {
       "types": ["StateChange"],
       "queue": "icinga2_StateChange_all"
      },
     "collector_config": {
       "event_type": "icinga2_StateChange_all",
       "payload": {
         "response": "${@}"
       }
     }
   }


.. note:: Based on the `Icinga 2 Event Streams documentation
   <https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#event-streams>`_,
   multiple HTTP clients can use the same queue name as long as they
   use the same event types and filter.

After configuring the streams, restart tornado_icinga2_collector resource.
