.. _ebp-verification-advance-concepts:

Blockchain Verification
~~~~~~~~~~~~~~~~~~~~~~~

In order to ensure the underlying blockchain was not altered or corrupted,
you can use :command:`verify` command provided by El Proxy. Prior to running
the command, you need to perform the :ref:`verification setup <ebp-setup-verify>`.

.. topic:: Running verification from a DPO machine

   When performing verification one should have access to the initial key.
   However, in case of storing the initial key on a NetEye machine, there is still
   a risk for the blockchain to be altered without this being noticed:
   users having access to a NetEye machine (e.g. the NetEye administrator) could use
   the initial key to rewrite the history of the blockchain.

   For this reason it is strongly recommended to store the initial key only on a DPO machine
   and run the verification from there. This would serve as a mitigation method for a risk
   of corruption, and guarantee the integrity of the blockchain data.

The command retrieves signature data of each log stored in the Elasticsearch blockchain,
recalculates the hash of each log and compares it with the one stored in the signature,
reporting a CorruptionId for all non-matching logs of the blockchain.
The `CorruptionId` can then also be used with the :command:`acknowledge` command, as explained in section
:ref:`acknowledging_blockchain_corruptions`.

By default, the command verifies always two batches in parallel, however the argument
``--concurrent-batches`` allows the user to increase the amount of parallel workers.

The flowchart depicted in figure :numref:`figure-elproxy-verification-overview` provides a detailed view
of the operations performed during the verification process and the reporting of discovered corruptions.

.. _figure-elproxy-verification-overview:

.. figure:: /feature-modules/elastic-stack/img/verification-overview.jpg

   El Proxy verification process Overview

For more information on the verify command and its parameters, please consult the
associated :ref:`El Proxy commands section <ebp-commands>`.


.. _elproxy-verification-and-retention-policies:

.. rubric:: Verification and Retention Policies

The blockchain stored in Elasticsearch is subject to a specific retention policy which defines how long the logs
will be kept. When the logs reach their maximum age, Elasticsearch deletes them. If the logs inside the
blockchain get deleted as an effect of the retention policy, it is impossible to verify the blockchain from start to
end. For this reason, El Proxy must consider the retention policy and thus start verifying the blockchain
from the first present log.

El Proxy uses a so-called Blockchain State History (:abbr:`BSH (Blockchain State History)`) file to retrieve the
first present log inside the
blockchain. This file is updated on every successful verification and contains one entry for each day that
specifies the first and last iteration of the indexed logs for that day.

When the :command:`verify` command is executed,
El Proxy fetches the BSH file searching for the entry corresponding to the
first day that still contains all
the logs at the moment of the verification. If the entry is present, El Proxy can retrieve the first
iteration for that day and start the verification from the iteration specified in the entry. If the entry is
not present or El Proxy cannot get the first iteration from the BSH,
then El Proxy directly queries
Elasticsearch to retrieve the first present log.

Note that although querying Elasticsearch is an option,
the only way for El Proxy to verify the blockchain's integrity is to retrieve the first iteration from the
BSH. For this reason, if the first log is retrieved by querying Elasticsearch,
the :command:`verify` command will throw
a warning after the successful verification. For more information about warnings and errors that could
appear during the verification, please consult :ref:`associated section <elproxy-verification-warnings-and-errors>`.

.. _elproxy-verification-and-duplicate-iterations:

.. rubric:: Verification and Duplicate Iterations

When verifying a blockchain, under particular circumstances the El Proxy verification may encounter multiple
logs with the same iteration. This may happen for example when some logs end up in DLQ despite being actually
indexed in Elasticsearch (e.g. due to an Elasticsearch internal server error) and then the :command:`dlq recover` command
is issued after the Elasticsearch data stream rollover has already been executed.

When multiple logs have the same iteration, the verification will verify each log with duplicate iteration and will
report a corruption only if one of these logs is corrupted. If no corruption is present, the verify will anyway report
:ref:`a warning <elproxy-verification-and-duplicate-iterations>` to signal the presence of duplicate logs.

Imagine for example that the following iterations are present in the blockchain:
``1, 2(a), 2(b), 3``. The verification will verify the log ``2(a)`` by checking its **hash**, and ensuring its **previous hash**
matches the **hash** of iteration ``1``. Then, in the exact same way, it will verify the log ``2(b)`` (by checking its **hash**,
and ensuring its **previous hash** matches the hash of iteration ``1``).

First Iteration Retrieval
``````````````````````````
During the verification process, one of the first steps performed by El Proxy is the retrieval of
the **first expected iteration**. This aspect is particularly important when considering the retention policies,
since the first expected iteration needs to be calculated based on the logs that we still expect to find
in Elasticsearch. The following diagram outlines the first steps of the verification process which involve
the retrieval of the first iteration and the various errors or warnings raised.

.. _figure-elproxy-verification-first-iteration:

.. figure:: /feature-modules/elastic-stack/img/elproxy-blockchain-verification-first-iteration.jpg

   El Proxy verification process - First iteration retrieval.

   (*) for more information please refer to the following `Must Exists vs May Exist` section

.. _elproxy-verification-must-exist-vs-may-exist:

.. topic:: Must Exist vs May Exist

   When retrieving the first expected iteration, we distinguish two possible cases:

	- El Proxy *must* find the first expected iteration in the blockchain to complete the verification successfully (*Must Exist*)
	- El Proxy *may* find the first expected iteration in the blockchain to complete the verification successfully (*May Exist*)

	The difference between these two alternatives resides in how El Proxy is able to calculate the
	first expected iteration, taking into account that not all days have indexed logs.

    Given the first date within the retention period, El Proxy calculates the first expected iteration.

    1. In a *Must Exist* case El Proxy extracts the iteration of the first log indexed on the first date within the retention period.
       If no logs were indexed on that particular date, El Proxy would look to extract the first iteration on each
       consequent day, moving forward till the end of the blockchain retention period.

    2. Still, if no indexed logs were found within retention period, i.e. *May Exist*, El Proxy switches to backward scanning,
       searching for indexed logs on dates precedent to retention policy until it succeeds finding one to extract the first iteration.


.. rubric:: Verification with Retries

The **elasticsearch-indexing-delay** parameter of the :command:`verify` command can help when the Blockchain
subject to verification contains recently created logs. The reason is that Elasticsearch might take some time
to index a document; therefore, El Proxy could be trying to verify a batch in which some logs are missing.
In order to avoid the erroneous failure of the verification, if El Proxy detects that some logs could be missing
because of an indexing delay of Elasticsearch, it repeats the whole batch verification.
The parameter **elasticsearch-indexing-delay** defines the maximum allowed time in seconds
for Elasticsearch to index a document. If Elasticsearch takes more than **elasticsearch-indexing-delay**
seconds to index a log, the verification will fail (see :command:`verify` command in :ref:`ebp-commands`).
If we name **timestamp_next_log** the timestamp of the next present log and **timestamp_verification** the timestamp
of the log verification, El Proxy considers a log as missing due to Elasticsearch indexing delay whenever
:command:`timestamp_next_log > timestamp_verification - elasticsearch-indexing-delay`.
It is worth noticing that El Proxy will always retry the batch verification for a finite amount of time, corresponding
to **elasticsearch-indexing-delay** seconds in the worst case.

.. _elproxy-verification-warnings-and-errors:

Warnings and Error Codes
`````````````````````````
The verification process may complete successfully, emit an error or a warning. Errors thrown by El Proxy lead to a failed
verification, while warnings may appear also on successful completion.

The following table reports the **warnings** that could be reported by El Proxy during the verification process,
along with the possible causes and actions that can be taken to address the issue.

.. _table-elproxy-verification-warning-untrusted-first-iteration-no-state-history:

.. list-table::
   :header-rows: 1
   :widths: 200 200 200

   * - Warning Code
     - Description
     - Actions

   * - Untrusted First Iteration No State History
     - The first iteration found on the blockchain cannot be trusted because it differs from iteration
       zero, and the blockchain state history file was not found.
       Thus, El Proxy cannot confirm that the iterations preceding the one that was found
       are missing. After the first successful blockchain verification,
       the blockchain state history file will be created, and the warning will disappear.
       For more info on this topic, refer to the
       :ref:`elproxy-verification-and-retention-policies` section.
     - Manually investigate the blockchain to confirm that all previous iterations
       are expected to be missing and re-run the verification.

.. _table-elproxy-verification-warning-untrusted-first-iteration-insufficient-info:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Untrusted First Iteration Insufficient Info
     - The first iteration found on the blockchain cannot be trusted because it differs from iteration
       zero, and the blockchain state history file does not contain enough information to assess the expected
       first iteration of the blockchain. This warning is thrown when the time between two consecutive verifications
       exceeds the retention period of the Elasticsearch indices where the blockchain is stored, thus
       leading to an outdated blockchain state history file.
     - Reduce the time between consecutive verifications by increasing the verification frequency.
       For more info on how to modify the verification frequency, refer to the :ref:`ebp-verify-neteye-setup`
       section.

.. _table-elproxy-verification-warning-unknown-first-iteration:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Unknown First Iteration
     - The blockchain is empty, and the blockchain state history file does
       not contain any information about the expected first iteration. This warning is thrown on two
       occasions:

       * the blockchain is new and no logs have been indexed yet
       * all logs have been deleted from the blockchain before any successful verification

     - Ensure that the blockchain is empty because it is new, and wait for the first log to be indexed
       before running the verification again.

.. _table-elproxy-verification-warning-empty-blockchain:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Empty Blockchain
     - The first expected iteration and all the subsequent iterations were not found in the blockchain.
       This warning differs from the :code:`Unknown First Iteration` warning. In this case, the first
       expected iteration was retrieved from the blockchain state history file meaning that the blockchain could
       contain some logs with iterations preceding the retrieved first expected iteration. This warning
       can be thrown for two reasons:

       * no logs have been indexed on the blockchain for at least the amount of time corresponding to the retention
         period of the blockchain
       * all the logs that were indexed after the last successful verification have been deleted

     - Ensure that the absence of newly indexed logs in the blockchain is expected, and wait for some logs
       to be indexed before running the verification again.

.. _table-elproxy-verification-warning-duplicate-iteration:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Duplicate Log Found
     - The verification found one or more logs that are duplicate, meaning that they have the same iteration and
       also have a valid signature. The warning also specifies the iterations for which multiple logs
       are present. For more information see also: :ref:`elproxy-verification-and-duplicate-iterations`.

     - Take note of the first reported duplicate iteration, access Elasticsearch (via Kibana for example), find the logs
       in the relative blockchain that have this iteration, and remove all of them **except one** (to maintain the validity
       of the blockchain).

       Repeat the operation for each of the reported duplicate iterations.

Similarly, the following table reports the **errors** that could be reported by El Proxy during the verification process,
along with the possible causes and actions that can be taken to address the issue.

.. _table-elproxy-verification-error-empty-blockchain-corruption:

.. list-table::
   :header-rows: 1
   :widths: 200 200 200

   * - Error Code
     - Description
     - Actions

   * - Empty Blockchain Corruption
     - The blockchain is empty, and the first expected iteration was not found, leading to a failed verification.
       El Proxy expected the first iteration based on the blockchain state history file written during the
       last successful blockchain verification. This error can be thrown if the blockchain
       state history file is corrupted or if all the logs have been deleted from the blockchain.
     - Ensure that the blockchain state history file has not been corrupted, then manually investigate
       the cause of the missing logs.

.. _table-elproxy-verification-error-log-verification-bad-iteration:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Log Verification Bad Iteration
     - The iteration of a log is wrong.
       This error could be thrown in case one or several consecutive logs are missing,
       or if one or several logs iterations have been modified.
     - Investigate the problem's cause. If the missing logs are present in the :ref:`DLQ <dead_letter_queue>`
       you can try to recover them with the :command:`dlq recover` command.
       In case the logs cannot be recovered, you can always
       acknowledge the error's corruption id with the :command:`elastic-blockchain-proxy acknowledge`
       command. After these operations you can run the verification again.

.. _table-elproxy-verification-error-log-verification-wrong-previous-hash:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Log Verification Wrong Previous Hash
     - The previous hash of a log does not match the hash of the previous log.
       This error can be thrown if the log's previous hash was modified.
     - Ensure to investigate the problem's cause and acknowledge the error's
       corruption id with the :command:`elastic-blockchain-proxy acknowledge`
       command. Then run the verification again.

.. _table-elproxy-verification-error-log-verification-wrong-hash:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Log Verification Wrong Hash
     - The hash of a log does not match the hash corresponding to its content.
       This error can be thrown if the log's content is modified.
     - Same as above.

.. _table-elproxy-verification-error-acknowledgement-verification-wrong-previous-hash:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Acknowledgement Verification Wrong Previous Hash
     - The previous hash of the acknowledgement for a corruption ID does not
       match the hash of the log right before the corrupted log. This error can be thrown
       if the previous hash of the acknowledgement has been modified.
     - Delete the acknowledgement and run the verification again.

.. _table-elproxy-verification-error-acknowledgement-verification-wrong-hash:

.. list-table::
   :header-rows: 0
   :widths: 200 200 200

   * - Acknowledgement Verification Wrong Hash
     - The hash of the acknowledgement for a corruption ID does not
       match the expected hash. This error can be thrown if the acknowledgement
       content has been modified.
     - Same as above.
