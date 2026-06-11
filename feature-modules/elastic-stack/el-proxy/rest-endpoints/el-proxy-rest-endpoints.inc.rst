
The following `REST <https://en.wikipedia.org/wiki/Representational_state_transfer>`__ endpoints
are used to receive data and expose the details of El Proxy status and metrics:

-  **log** endpoint:

   -  description: Receives and processes a single log message in JSON
      format
   -  path : **/api/log**
   -  method: POST
   -  request type: **JSON**
   -  request example:

      .. code:: json

         {
            "entry": "A log message"
         }

   -  response: The HTTP Status code 200 is used to signal a
      successful processing. Other HTTP status codes indicate a failure.

-  **log_batch** endpoint:

   -  description: Receives and processes an array of log messages in
      JSON format
   -  path : **/api/log_batch**
   -  method: POST
   -  request type: **JSON**
   -  request example:

      .. code:: json

         [
              {
                  "entry": "A log message",
                 "other": "Additional values...",
                 "EBP_METADATA": {
                    "agent": {
                       "type": "auditbeat",
                       "version": "7.10.1"
                    },
                    "customer": "neteye",
                    "retention": "6_months",
                    "blockchain_tag": "0",
                    "event": {
                       "module": "elproxysigned"
                    },
                    "pipeline": "my_es_ingest_pipeline"
                 }
              },
              {
                  "entry1": "Another log message",
                 "entry2": "Another log message",
                 "EBP_METADATA": {
                    "agent": {
                       "type": "auditbeat",
                       "version": "7.10.1"
                    },
                    "customer": "neteye",
                    "retention": "6_months",
                    "blockchain_tag": "0",
                    "event": {
                       "module": "elproxysigned"
                    },
                    "pipeline": "my_es_ingest_pipeline"
                 }
              },
              {
                 "entry": "Again, another message",
                "EBP_METADATA": {
                   "agent": {
                      "type": "auditbeat",
                      "version": "7.10.1"
                   },
                   "customer": "neteye",
                   "retention": "6_months",
                   "blockchain_tag": "0",
                   "event": {
                      "module": "elproxysigned"
                   }
                }
              }
         ]

   -  response: The HTTP Status code 200 is used to signal a
      successful processing. Other HTTP status codes indicate a failure.

-  **DeadLetterQueue status** endpoint:

   -  description: Returns the status of the Dead Letter Queue. The status
      contains information on whether the DLQ is empty or not.
   -  path : **/api/v1/status/dlq**
   -  method: GET
   -  response: The status of the DLQ in JSON format.
   -  response example:

      .. code:: json

         {
           "empty": true
         }

-  **KeysBackup status** endpoint:

   -  description: Returns the status of the keys backup. The status
      contains information on whether any key backup is present in the
      ``{data_backup_dir}`` folder.
   -  path : **/api/v1/status/keys_backup**
   -  method: GET
   -  response: The status of the Keys Backup in JSON format.
   -  response example:

      .. code:: json

         {
           "empty": true
         }

-  **Metrics** endpoint:

   -  description: Returns the metrics collected by El Proxy since the
      last restart.
   -  path : **/api/v1/metrics/prometheus**
   -  method: GET
   -  response: The metrics collected by El Proxy since the last service restart
   -  response example:

      .. code:: text

         # HELP el_proxy_response_time Response time of El Proxy for signing requests
         # TYPE el_proxy_response_time histogram
         el_proxy_response_time_bucket{service_name="elproxy",le="0.05"} 8
         el_proxy_response_time_bucket{service_name="elproxy",le="0.1"} 8
         el_proxy_response_time_bucket{service_name="elproxy",le="0.2"} 9
         el_proxy_response_time_bucket{service_name="elproxy",le="0.5"} 10
         el_proxy_response_time_bucket{service_name="elproxy",le="1"} 10
         el_proxy_response_time_bucket{service_name="elproxy",le="2"} 11
         el_proxy_response_time_bucket{service_name="elproxy",le="5"} 11
         el_proxy_response_time_bucket{service_name="elproxy",le="+Inf"} 11
         el_proxy_response_time_sum{service_name="elproxy"} 1.51250216
         el_proxy_response_time_count{service_name="elproxy"} 11
         # HELP processed_logs Number of processed logs
         # TYPE processed_logs counter
         processed_logs{result="dlq",service_name="elproxy"} 2
         processed_logs{result="failure",service_name="elproxy"} 2
         processed_logs{result="indexed",service_name="elproxy"} 18
         # HELP processed_requests Number of processed requests
         # TYPE processed_requests counter
         processed_requests{result="failure",service_name="elproxy"} 1
         processed_requests{result="success",service_name="elproxy"} 10
         # HELP received_logs Number of received logs
         # TYPE received_logs counter
         received_logs{retention="6_months",service_name="elproxy",tag="0",tenant="mycustomer"} 22
         # HELP received_requests Number of received requests
         # TYPE received_requests counter
         received_requests{service_name="elproxy"} 11
         # HELP write_logs_elasticsearch_response_time Elasticsearch response time for requests writing logs
         # TYPE write_logs_elasticsearch_response_time histogram
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="0.05"} 14
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="0.1"} 14
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="0.2"} 15
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="0.5"} 16
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="1"} 16
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="2"} 16
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="5"} 16
         write_logs_elasticsearch_response_time_bucket{service_name="elproxy",le="+Inf"} 16
         write_logs_elasticsearch_response_time_sum{service_name="elproxy"} 0.450198966
         write_logs_elasticsearch_response_time_count{service_name="elproxy"} 16

The following metrics are available:

-  ``received_requests``: number of received requests made to **/api/log** and **/api/log_batch**
-  ``processed_requests``: number of processed requests. The following attributes are used:

   - ``result``: the result of the processing of one request

     - ``success``: in case the request response was OK, meaning that El Proxy returned 200 to Logstash.
       Note that a request is also considered to be successful if some of the logs failed to be indexed on
       Elasticsearch but were successfully written in the DLQ.
     - ``failure``: in case the request response was other than OK.

   - ``failure_type``: the failure type of the processing of one request

     - ``none``: the request was processed successfully. In this case, ``result`` is always ``success``
     - ``elasticsearch_error``: the request to El Proxy failed due to an error during a request to
       Elasticsearch (e.g. the Elasticsearch request went in timeout or Elasticsearch returned a 500
       Internal Server Error)
     - ``file_system_error``: the request failed because of a file system error. This failure
       is often related to misconfigured permissions on the |ne| machine
     - ``dlq_error``: the request triggered the writing of the logs in DLQ, and the writing operation
       failed
     - ``full_queue_error``: the request failed because the internal communication channel of El Proxy
       between the requests-producer and the requests-consumer is either closed or saturated
     - ``internal_error``: the request failed because of a general internal error
     - ``unexpected_error``: the request failed because of an unexpected error

-  ``received_logs``: number of received logs. The following attributes are used to group the number
   of received logs by their target blockchain

   - ``tenant``: the tenant of the target blockchain for a given log
   - ``retention``: the retention of the target blockchain for a given log
   - ``tag``: the tag of the target blockchain for a given log

- ``processed_logs``: number of processed logs. The following attribute is used:

  - ``result``: the result of the processing of one log

    - ``indexed``: the log was successfully indexed on Elasticsearch
    - ``dlq``: the log was successfully written in DLQ
    - ``failure``: the log was neither indexed in Elasticsearch, neither written in DLQ, due to some errors

- ``el_proxy_response_time``: the time taken by El Proxy to receive and process a request
- ``elasticsearch_requests``: the number of indexing requests made to Elasticsearch. The following attributes are used:

  - ``result``: the result of the request. It can be one of the following

    - ``success``: the request was successful. Every log of the request was indexed in Elasticsearch
    - ``partial_failure``: the request was not completely successful. One or more logs of the request failed
      to be indexed in Elasticsearch
    - ``failure``: the request failed. Zero logs were indexed in Elasticsearch

  - ``retry_reason``: the reason for which a previous non-completely-successful request was retried.
    It can be one of the following

    - ``none``: the request was not a retry request caused by a previous non-completely-successful request
    - ``global``: the request is the retry of a previous failed request. This request is the same as the
      previously failed one. Such retry requests could happen for example if the Elasticsearch is having some
      problems, or is not reachable
    - ``pipeline``: the request is the retry of a previously partially-failed request. In this request, the
      Elasticsearch pipeline (if present) has been removed from the logs to bypass possible pipeline errors
    - ``malformed_logs``: the request is the retry of a previously non-completely-successful request.
      In this request, each log has been reduced to its bare minimum removing all the fields which are not
      signed by El Proxy to bypass possible errors due to the malformed content of the log

- ``write_logs_elasticsearch_response_time``: the time taken by Elasticsearch to process a request
