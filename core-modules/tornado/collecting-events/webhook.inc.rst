.. _tornado-webhook-collector-exec:

Webhook
~~~~~~~

Out of the box the Webhook Collector is available on the |ne| Master and, if present, on Satellites,
and the webhooks are to be configured.

On startup, it creates a dedicated REST endpoint for each configured
webhook. Calls received by an endpoint are processed by the embedded
JMESPath Collector that uses them to produce Tornado Events. In
the final step, the Events are forwarded to the Tornado Engine through
the configured connection type.

Webhooks in Tornado Webhook Collector are defined by JSON files, which
are stored in the folder ``/neteye/shared/tornado_webhook_collector/conf/webhooks/``.

For each webhook, you must provide the following values in order to successfully
create an endpoint:

-  *id*: The webhook identifier. This will determine the path of the
   endpoint; it must be unique per webhook.
-  *token*: A security token that the webhook issuer has to include in
   the URL as part of the query string (see the example at the bottom of
   this page for details). If the token provided by the issuer is
   missing or does not match the one owned by the Collector, then the
   call will be rejected and an HTTP 401 code (UNAUTHORIZED) will be
   returned.
-  *max_payload_size*: (Optional) The maximum size of the payload that the
   webhook can accept. If the payload exceeds this size, the call will be
   rejected and an HTTP 413 code (PAYLOAD TOO LARGE) will be returned.
   The value can be specified as a number in bytes, or as a string using human-readable units:
   ``b`` (bytes), ``k`` (kilobytes), ``m`` (megabytes), ``g`` (gigabytes), ``p`` (petabytes), or ``e`` (exabytes).
   If not specified, the default value is 5MB.
-  *collector_config*: The transformation logic that converts a webhook
   JSON object into a Tornado Event. It consists of a JMESPath Collector
   configuration.
-  *event_type*: A mandatory field to define the event type of the Tornado
   event generated based on a call received by an endpoint.


.. rubric:: Configuration

The executable configuration is based partially on configuration files, and partially
on command line parameters.

The available startup parameters are:

- **config-dir**: The filesystem folder from which the collector configuration is read.
  The default path is ``/etc/tornado_webhook_collector/``.
- **webhooks-dir**: The folder where the Webhook configurations are saved in JSON format;
  this folder is relative to the `config_dir`. The default value is ``/webhooks/``.

In addition to these parameters, the following configuration entries are available in the
file ``config-dir'/webhook_collector.toml``:

- **logger**:

  - **level**: The Logger level; valid values are trace, debug, info, warn, and error.
  - **stdout**: Determines whether the Logger should print to standard output. Valid values are true and false.
  - **file_output_path**: A file path in the file system; if provided, the Logger will append any output to it.

- **webhook_collector**:

  - **tornado_event_socket_ip**: The IP address where outgoing events will be written. This should be the address
    where the Tornado Engine listens for incoming events. If present, this value overrides what specified
    by the `tornado_connection_channel` entry. *This entry is deprecated and will be removed in the next release of tornado.
    Please, use the * `tornado_connection_channel` *instead*.
  - **tornado_event_socket_port**: The port where outgoing events will be written. This should be the port
    where the Tornado Engine listens for incoming events. This entry is mandatory if `tornado_connection_channel` is set to `TCP`.
    If present, this value overrides what specified by the `tornado_connection_channel` entry.
    *This entry is deprecated and will be removed in the next release of tornado. Please, use the* `tornado_connection_channel` *instead*.
  - **message_queue_size**: The in-memory buffer size for Events. It makes the application resilient to errors or
    temporary unavailability of the Tornado connection channel. When the connection on the channel is restored,
    all messages in the buffer will be sent. When the buffer is full, the collector will start discarding older messages first.
  - **server_bind_address**: The IP to bind the HTTP server to.
  - **server_port**: The port to be used by the HTTP Server.
  - **tornado_connection_channel**: The channel to send events to Tornado. It contains the set of entries required
    to configure a Nats or a TCP connection. Beware that this entry will be taken into account only if `tornado_event_socket_ip`
    and `tornado_event_socket_port` are not provided.

    - In case of connection using Nats, these entries are mandatory:

      - **nats.client.addresses**: The addresses of the NATS server.
      - **nats.client.auth.type**: The type of authentication used to authenticate to NATS (Optional.
        Valid values are `None` and `Tls`. Defaults to `None` if not provided).
      - **nats.client.auth.certificate_path**: The path to the client certificate (in `.pem` format) that will be used
        for authenticating to NATS. (Mandatory if `nats.client.auth.type` is set to `Tls`).
      - **nats.client.auth.private_key_path**: The path to the client certificate private key (in `.pem` format) that
        will be used for authenticating to NATS.
      - **nats.client.auth.path_to_root_certificate**: The path to a root certificate (in `.pem` format) to trust in addition
        to system's trust root. May be useful if the NATS server is not trusted by the system as default. (Optional, valid
        if `nats.client.auth.type` is set to `Tls`).
      - **nats.subject**: The NATS Subject where tornado will subscribe and listen for incoming events.
    - In case of connection using TCP, these entries are mandatory:

      - **tcp_socket_ip**: The IP address where outgoing events will be written. This should be the address where
        the Tornado Engine listens for incoming events.
      - **tcp_socket_port**: The port where outgoing events will be written. This should be the port where
        the Tornado Engine listens for incoming events.

  - **workers**: The number of worker threads to be used by the webhook collector. This value must be a positive integer number.
    (Optional. If not specified, the number of workers will be calculated based on your machines core count).


The default **config-dir** value can be customized at build time by specifying the environment variable
TORNADO_WEBHOOK_COLLECTOR_CONFIG_DIR_DEFAULT. For example, this will build an executable that uses `/my/custom/path` as the default value:

.. code:: bash

   TORNADO_WEBHOOK_COLLECTOR_CONFIG_DIR_DEFAULT=/my/custom/path cargo build

An example of a full startup command is:

.. code:: bash

   ./tornado_webhook_collector --config-dir=/tornado-webhook-collector/config

In this example the Webhook Collector starts up and then reads the configuration from the /tornado-webhook-collector/config directory.

Assuming the webhook request is:

.. code:: bash

   curl -v -X POST https://<neteye.master>/tornado/webhook/event/ups?token=my-secret-token -d '{"ups": "001", "type": "critical", "message": "Battery level critical"}'


the configuration file :file:`ups_notifications.conf` would contain the following values:

.. code:: json

   {
    "id": "ups",
    "token": "my-secret-token",
    "collector_config": {
      "event_type": "ups_notification",
      "payload": {
        "source": "${@}"
       }
     }
   }


the event received is:

.. code:: json

   {
    "created_ms": 1713266596490,
    "metadata": {
      "tenant_id": "master",
    },
    "payload": {
      "source":{
        "message":"Battery level critical",
        "type":"critical",
        "ups":"001"
        }
      }
    },
    "type": "ups_notification"
   }


Detailed information on how to configure webhooks in Tornado can the
found inside the official `Tornado documentation
<https://github.com/WuerthPhoenix/tornado/>`__; in particular, the
`Webhook Collector documentation
<https://github.com/WuerthPhoenix/tornado/blob/develop/tornado/webhook_collector/README.md>`__
describes the architecture of the Webhook Collector and its
configuration parameters and options.
