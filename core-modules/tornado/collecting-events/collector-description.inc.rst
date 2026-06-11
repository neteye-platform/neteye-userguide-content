Overview
~~~~~~~~

Tornado provides a number of preconfigured Collectors that handle
inputs from various data sources:

#. Email Collector
#. Rsyslog Collector
#. Webhook Collector
#. Nats JSON Collector
#. Icinga 2 Collector
#. SNMP Trap Daemon Collector
#. SMS Collector

Most of the Tornado Collectors are functioning out of the box and do not require
manual configuration.
However, there are some, that may be configured to work in accordance with your needs.


.. _tornado-collector-event-properties:

The Event Type
``````````````

All collectors convert the external events they collect to Tornado internal events that are then
forwarded to the Tornado Processing Engine. These events share a similar base structure, but every
collector adds their own data to the event payload and sets a custom event type. An event consists
of the following properties:

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "type", "String", "The type/source, of the event. Dependent on the collector that creates the event"
    "created_ms", "Integer", "A timestamp in milliseconds, when the collector created the event"
    "payload", "Object", "A JSON object that contains all the data for the event"
    "metadata (Optional)", ":ref:`tornado-collector-event-metadata-properties`", "Metadata for the event"

.. note::

    The Tornado iterator node can also add a new field :command:`iterator` to
    the event, which is only present in the processing tree for the children of
    an iterator node.


.. _tornado-collector-event-metadata-properties:

EventMetadata
+++++++++++++

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "tenant_id (Optional)", "String", "If the event is received over Nats, the Tornado collector will set this field, dependent on the sender."
..  Don't document trace context for now
..  "trace_context (Optional)", "Object", "The OpenTelemetry trace-context"
