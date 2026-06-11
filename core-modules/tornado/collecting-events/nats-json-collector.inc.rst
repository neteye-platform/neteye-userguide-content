.. _tornado-nats-json-collector-exec:

Tornado Nats JSON
~~~~~~~~~~~~~~~~~

The Nats JSON Collector is a standalone Collector that listens for JSON
messages on Nats topics, generates Tornado Events, and sends them to the
Tornado Engine.

On startup, it connects to a set of topics on a Nats server. Calls
received are then processed by the embedded JMESPath Collector that uses them to produce Tornado Events.
In the final step, the Events are forwarded to the Tornado Engine through the
configured connection type.

For each topic, you must provide two values in order to successfully
configure them:

-  *nats_topics*: A list of Nats topics to which the Collector will
   subscribe.
-  *collector_config*: (Optional) The transformation logic that
   converts a JSON object received from Nats into a Tornado Event. It
   consists of a JMESPath Collector configuration.

.. rubric:: Topics configuration

Two startup parameters *config-dir* and *topics-dir* determine the path to the topic configurations,
and each topic is configured by providing *nats_topics* and *collector_config*.
**topics-dir** is the folder where the topic configurations are saved in JSON format. This folder is relative
to the *config_dir*. The default value is */topics/*.

An example of valid content for a Topic configuration JSON file is:

.. code:: json

   {
    "nats_topics": ["simple_test_one", "simple_test_two"],
    "collector_config": {
      "event_type": "${content.type}",
      "payload": {
        "ref": "${content.ref}",
        "repository_name": "${repository}"
      }
    }
  }

With this configuration, two subscriptions are created to the Nats topics simple_test_one and simple_test_two.
Messages received by those topics are processed using the collector_config that determines the content of the Tornado Event associated with them.

It is important to note that, if a Nats topic name is used more than once, then the collector will perform multiple subscriptions accordingly. This can happen if a topic name is duplicated into the nats_topics array or in multiple JSON files.
So for example, with this JSON message is received:

.. code:: json

   {
      "content": {
         "type": "content_type",
         "ref": "refs/heads/master"
      },
      "repository": {
         "id": 123456789,
         "name": "webhook-test"
      }
   }

then the resulting Event will be:

.. code:: json

   {
      "type": "content_type",
      "created_ms": 1554130814854,
      "payload": {
         "ref": "refs/heads/master",
         "repository": {
            "id": 123456789,
            "name": "webhook-test"
         }
      }
   }

.. rubric:: Default values

The collector_config section and all of its internal entries are optional.
If not provided explicitly, the collector will use these predefined values:

- When the *collector_config.event_type* is not provided, the name of the Nats topic that sent the message is used as Event type.
- When the *collector_config.payload* is not provided, the entire source message is included in the payload of the generated Event with the key data.

Consequently, the simplest valid topic configuration contains only the nats_topics:

.. code:: json

   {
      "nats_topics": ["subject_one", "subject_two"]
   }

The above one is equivalent to:

.. code:: json

   {
      "nats_topics": ["subject_one", "subject_two"],
      "collector_config": {
         "payload": {
          "data": "${@}"
         }
      }
   }

In this case the generated Tornado Events have *type* equals to the topic name and the whole source data in their payload.
