Health Checks
~~~~~~~~~~~~~

To make sure that the blockchain is sound and no errors occurred during the collection of the
logs, |ne| defines different Health Checks to alert the users of any irregularities that may
occur.

* ``logmanager-blockchain-creation-status`` : Makes sure that all logs are written correctly into the blockchain.

If this Health Check is CRITICAL, it means that some logs could not be written successfully to
Elasticsearch. The logs in question are then written to the Dead Letter Queue, where you can also
find the reason for which they could not be indexed in Elasticsearch.

To resolve this issue visit section :ref:`dead_letter_queue`

* ``logmanager-blockchain-keys`` :  Verifies that no backup of the Signature Key is present
  on the NetEye installation, to prevent any tampering on the blockchains from a compromised system.

Background and solution for this issue can be found in :ref:`elproxy_generation_of_signature_keys`.

* ``logmanager-blockchain-missing-elasticsearch-pipeline`` : Checks if any Elasticsearch
  ingest pipeline used to enrich logs is missing.

If a log was sent through Logstash with a non-existing pipeline, Elasticsearch will refuse
to persist the log and return with an error. As seen in Error Handling, the Health Check then
periodically queries Elasticsearch for logs with a corresponding tag and if found, set the status to CRITICAL.

To resolve this, remove the tags from the document in Elasticsearch.

Error Tags
``````````

If an error occurs, El Proxy may add a `tag <https://www.elastic.co/guide/en/ecs/current/ecs-base.html#field-tags>`__
to mark the log that caused the problem.

Currently, EL Proxy uses three tags to indicate such errors:

* ``_ebp_remove_pipeline`` : Will be added as seen in :ref:`this figure <figure-elproxy-error-handling>`
* ``_ebp_skip_previous_hash`` : Will be added to a log if the previous log hash was not available. You
  can have a look at :numref:`figure-elproxy-log-signing` to understand the circumstances under which this can happen.
* ``_ebp_dlq_recovered``: Will be added to a log if it was recovered from the Dead Letter Queue with
  the :command:`dlq recover` command.
