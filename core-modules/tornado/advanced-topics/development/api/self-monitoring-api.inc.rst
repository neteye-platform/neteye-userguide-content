
.. this is not necessary here. perhaps we can move it elsewhere as
   short intro

   Tornado API
   +++++++++++

   The Tornado API endpoints allow to interact with a Tornado instance.

   More details about the API can be found in the :ref:`Tornado backend
   documentation <tornado-backend>`.

Self-Monitoring API
-------------------

The monitoring endpoints allow you to monitor the health of Tornado.
They provide information about the status,
activities, logs and metrics of a running Tornado instance.
Specifically, they return statistics about latency, traffic, and
errors.

Available endpoints:

.. rubric:: Ping endpoint

This endpoint returns a simple message "pong - " followed by the current
date in ISO 8601 format.

Details:

-  name : **ping**
-  path : **/monitoring/ping**
-  response type: **JSON**
-  response example:

   .. code:: json

      {
        "message": "pong - 2019-04-12T10:11:31.300075398+02:00",
      }


.. rubric:: Metrics endpoint

This endpoint returns Tornado metrics in the `Prometheus text format <https://prometheus.io/docs/instrumenting/exposition_formats/#prometheus-text-format>`__
Details:

-  name : **metrics/prometheus**
-  path : **/monitoring/v1/metrics/prometheus**
-  response type: **Prometheus text format**
-  response example:

   .. code:: text

      # HELP events_processed_counter Events processed count
      # TYPE events_processed_counter counter
      events_processed_counter{app="tornado",event_type="icinga_process-check-result"} 1
      # HELP events_processed_duration_seconds Events processed duration
      # TYPE events_processed_duration_seconds histogram
      events_processed_duration_seconds_bucket{app="tornado",event_type="icinga_process-check-result",le="0.5"} 1
      events_processed_duration_seconds_bucket{app="tornado",event_type="icinga_process-check-result",le="0.9"} 1
      events_processed_duration_seconds_bucket{app="tornado",event_type="icinga_process-check-result",le="0.99"} 1
      events_processed_duration_seconds_bucket{app="tornado",event_type="icinga_process-check-result",le="+Inf"} 1
      events_processed_duration_seconds_sum{app="tornado",event_type="icinga_process-check-result"} 0.000696327
      events_processed_duration_seconds_count{app="tornado",event_type="icinga_process-check-result"} 1
      # HELP events_received_counter Events received count
      # TYPE events_received_counter counter
      events_received_counter{app="tornado",event_type="icinga_process-check-result",source="http"} 1
      # HELP http_requests_counter HTTP requests count
      # TYPE http_requests_counter counter
      http_requests_counter{app="tornado"} 1
      # HELP http_requests_duration_secs HTTP requests duration
      # TYPE http_requests_duration_secs histogram
      http_requests_duration_secs_bucket{app="tornado",le="0.5"} 1
      http_requests_duration_secs_bucket{app="tornado",le="0.9"} 1
      http_requests_duration_secs_bucket{app="tornado",le="0.99"} 1
      http_requests_duration_secs_bucket{app="tornado",le="+Inf"} 1
      http_requests_duration_secs_sum{app="tornado"} 0.001695673
      http_requests_duration_secs_count{app="tornado"} 1

The following metrics are provided:

-  ``events_received_counter`` : total number of received events grouped by source (nats, tcp, http) and event type
-  ``events_processed_counter`` : total number of processed events grouped by event type
-  ``events_processed_duration_seconds_sum`` : total time spent for event processing
-  ``events_processed_duration_seconds_count`` : total number of processed events
-  ``invalid_events_received_counter`` : total number of received event with a not valid format. This can be caused, for example,
   by a not valid JSON representation or by the missing of mandatory fields
-  ``actions_received_counter`` : total number of received actions grouped by type
-  ``actions_processed_counter`` : total number of processed actions grouped by type and outcome (failure or success)
-  ``actions_processing_attempts_counter`` : total number of attempts to execute an action grouped by type and outcome (failure or success).
   This number can be greater than the total number of received actions because the execution of a single action can
   be attempted multiple times based on the defined Retry Strategy

Many other metrics can be derived from these ones, for example:

-  events_processed_counter - events_received_counter = events waiting to be processed
-  events_processed_duration_seconds_sum / events_processed_duration_seconds_count = mean processing time for an event
