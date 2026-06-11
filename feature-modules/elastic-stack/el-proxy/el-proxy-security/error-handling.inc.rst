.. _el_proxy_error_handling:

Error Handling
~~~~~~~~~~~~~~

El Proxy implements two different recovery strategies in case of
errors.  The :ref:`first one <ebp-error-strategy-one>` is a simple
*retry* strategy used in case of unrecoverable communication errors
with Elasticsearch; the :ref:`second one <ebp-error-strategy-two>`,
instead, is used in case Elasticsearch has issues with processing some
of the sent logs and aims at doing whatever is possible in order to
successfully save those logs in Elasticsearch. Both strategies resolve to
the use of a Dead Letter Queue to deal with a widely known issue named
:ref:`backpressure <backpressure-in-el-proxy>`.

Figure :numref:`figure-elproxy-error-handling` shows how El Proxy
integrates the two aforementioned strategies,
which will be thoroughly explained in the next paragraphs.

.. _figure-elproxy-error-handling:

.. figure:: /feature-modules/elastic-stack/img/error-handling.png

   Error Handling in El Proxy

.. _backpressure-in-el-proxy:

.. topic:: Backpressure explained

   In the context of data pipelines, *backpressure* represents
   the situation where a resistance prevents a fast processing of the
   input data. In the long term this resistance causes the
   overload of the system, because inside a certain component of the system
   data arrive at a higher rate with respect to the output rate.

   In the context of the El Proxy, the resistance is represented by
   Elasticsearch refusing to index some specific logs because, for example,
   some unexpected fields inside a log lead to a failure in the Elasticsearch pipelines.
   This would cause El Proxy and Logstash to keep trying to index these logs
   without ever succeeding.

   The way El Proxy overcomes the backpressure is by:

   #. trying to modify the failing logs to avoid the indexing error
   #. writing these logs into a Dead Letter Queue, as explained in
      the :ref:`Strategy two <ebp-error-strategy-two>` section

   By performing these actions El Proxy can quickly process also
   the logs which cannot be indexed in Elasticsearch and so it
   guarantees that these logs will not cause the backpressure.


.. _ebp-error-strategy-one:

Strategy One: Bulk retry
````````````````````````

El Proxy implements an optional *retry* strategy to handle
communication errors with Elasticsearch; when enabled (see the
:ref:`Configuration <el-proxy-config-files>`), whenever a generic
error is returned by Elasticsearch, El Proxy will retry for a fixed
amount of times to resubmit the same logs to Elasticsearch.

This strategy permits to deal, for example, with temporary networking
issues without resolving to writing the logs in DLQ.

Nevertheless, while this can be useful in dealing with a large set of
use cases, it should also be used very carefully. In fact, due to the
completely sequential nature of the blockchain processing, a too
high number of retries could lead to an ever growing queue of logs
waiting to be processed while El Proxy is busy
with processing over and over again the same failed logs.

In conclusion, whether it is better to let El Proxy
fail fast or retry more times is a decision that needs a careful,
case-by-case analysis.

.. _ebp-error-strategy-two:

Strategy Two: Single Log Reprocessing
`````````````````````````````````````

In some cases, Elasticsearch can correctly process the log bulk
request but it can fail to index some of the contained logs. In this
situation, El Proxy reprocesses only those failed logs and follows a
procedure aimed at ensuring that each signed log will be indexed in Elasticsearch.

The procedure is the following:

#. If the log indexing error is caused by the fact that the Elasticsearch
   Ingest Pipeline specified for the log does not exist in Elasticsearch, then:

   #. El Proxy tries to index again the log **without** specifying any Ingest Pipeline.
      When doing this, El Proxy will also add the tag ``_ebp_remove_pipeline`` to the Elasticsearch document,
      so that once the log is indexed, it will be visible that the Ingest Pipeline
      was bypassed during the document indexing.

   #. If the indexing fails again, El Proxy proceeds with step 2 of this procedure.

#. El Proxy removes from the failed log all the fields not required by the signature process
   and sends the modified document to Elasticsearch. Note that also all the document tags
   (including the ``_ebp_remove_pipeline`` tag) will be removed from the document.

   This strategy is based on the assumption that the indexing of the specific log fails due
   to incompatible operations requested by a pipeline. For example, a pipeline could attempt to
   lowercase a non-string field causing the failure.
   Consequently, resubmitting the log without the problematic fields could lead to a
   successful indexing.

.. _ebp-error-writing-logs-in-dlq:

Writing Logs to DLQ
```````````````````

If both Strategy One and Two fail to index logs in Elasticsearch,
then El Proxy will dump the failed logs to
a *Dead Letter Queue* on the filesystem and it will send an **ok**
response to Logstash.  Usually, when this happens, the blockchain will
be in an incoherent state caused by holes in the numeric
progression iteration. The administrator is in charge of investigating the issue and,
if possible, recover the non-indexed logs with the :command:`dlq recover`
command or manually acknowledge the problem.

As mentioned before, the *Dead Letter Queue* is saved on the filesystem inside the
``{dlq_dir}`` folder as set in the configuration file.
The *Dead Letter Queue* consists of a set of text files in `Newline delimited JSON <http://ndjson.org/>`__ format
with each row in a file representing a failed log.
These files are grouped by customer, data stream name and date following the naming convention:

``{dlq_dir}/{customer}/{data_stream}-{date}.ndjson``

where ``{data_stream}`` is the name of the Elasticsearch data stream where the failed logs
were supposed to be written and ``{date}`` is the date when logs were written to DLQ.

If the :command:`dlq recover` command is used to recover the logs, the tag ``_ebp_dlq_recovered``
is added to each log at the moment of indexing in Elasticsearch. The successfully
recovered logs are moved inside the ``{dlq_recovered}`` folder as
set in the configuration file. The logs that failed to be recovered remain
unchanged in the ``{dlq_dir}`` folder.

If the administrator wants to move the logs from the ``{dlq_dir}`` manually,
the convention is to move them inside a folder called ``{archive_dlq}``.
The parent folder of each :abbr:`NDJSON (Newline Delimited JSON)` file should
not be changed. So the naming convention for a manually moved file should be:

``{dlq_recovered}/{customer}/{data_stream}-{date}.ndjson``

Each entry in the *Dead Letter Queue* file contains the original log whose indexation failed and, if present,
the Elasticsearch error that caused the failure. For example:

.. code:: json

   {
      "document":  {
         "value": "A log message",
         "origin": "linux-apache2",
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
         },
         "ES_BLOCKCHAIN": {
            "fields": {
               "value": "A log message",
               "origin": "linux-apache2"
            },
            "hash": "HASH_OF_THE_CURRENT_LOG",
            "previous_hash": "HASH_OF_THE_PREVIOUS_LOG",
            "iteration": 11,
            "timestamp_ms": 123456789
         }
      },
      "el_error": {
            "reason": "field [string_field] of type [java.lang.Integer] cannot be cast to [java.lang.String]",
            "type": "illegal_argument_exception"
      }
   }
