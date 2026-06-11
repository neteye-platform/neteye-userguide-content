.. _dead_letter_queue:

Handling Logs in Dead Letter Queue
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Logs which could not be indexed in Elasticsearch are written
in the :abbr:`DLQ (Dead Letter Queue)` of El Proxy.

Since events written in the DLQ are not part of the secured
blockchain indices, the |ne| Administrator needs to recover or explicitly
acknowledge the presence of the events in the DLQ.

As soon as any log ends up in the DLQ, the Icinga2 service
**logmanager-blockchain-creation-neteyelocal** will enter in
*CRITICAL* state, indicating that some log could not be indexed
in the blockchain.

To recover the logs, the |ne| admin can use the :command:`dlq recover` command,
which will try to write the DLQ logs on Elasticsearch in their original Data Stream (see :ref:`ebp-how-data-stream-name-is-determined`).
If some of the logs cannot be recovered, they will remain in the DLQ.

.. warning:: To ensure that recovered logs are not kept in Elasticsearch longer than their retention
   period, please execute the :command:`dlq recover` command as soon as logs end up in DLQ.

   The retention of the logs starts from the moment when the logs are written in Elasticsearch, which
   means that if you for example recover logs from DLQ after one month, those logs will be kept in
   Elasticsearch one month more than the expected retention period.

If for any reason you cannot recover DLQ via the :command:`dlq recover` command, a blockchain corruption
will be reported by the next El Proxy verification because some logs are missing in the blockchain
saved on Elasticsearch. You can then acknowledge the corruption ID reported by the verification by
following the procedure in :ref:`handling_blockchain_corruptions`.

To clean up the DLQ and solve the CRITICAL state of ``logmanager-blockchain-creation-neteyelocal`` please:

#. Ensure the logs do not contain signs of malicious activities by inspecting the content of
   the DLQ log files :file:`/neteye/shared/elastic-blockchain-proxy/data/dlq/<tenant>/<filename>`,
   where <tenant> is the name of the tenant that generated the log, and <filename> is the name of the log file.

#. Delete the logs from the DLQ by deleting the log files in the directory
   :file:`/neteye/shared/elastic-blockchain-proxy/data/dlq/<tenant>`.
