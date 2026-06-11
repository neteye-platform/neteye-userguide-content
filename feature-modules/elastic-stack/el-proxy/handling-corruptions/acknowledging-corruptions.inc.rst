
The correct state of a blockchain can be checked by :ref:`running the verification process <ebp-setup-verify>`,
which provides, at the end of its execution, a report about the consistency of the
blockchain.

If everything is fine, the verification process will complete successfully, otherwise,
the execution report will contain a list of all errors encountered. For example:

.. container:: codeblock

   .. code::

      --------------------------------------------------------
      --------------------------------------------------------
      Verify command execution completed.
      Blockchain verification process completed with 2 errors.
      --------------------------------------------------------
      Errors detail:
      --------------------------------------------------------
      Log verification error 0 details:
       - Error code: Log Verification Bad Iteration
       - Error type: Missing logs
       - Missing logs from iteration 0 (inclusive) to iteration 1 (inclusive)
       - CorruptionId: eyJmcm9tX2l0ZXJhdGlvbiI6MCwidG9faXRlcmF0aW9uIjoxLCJwcmV2aW91c19oYXNoIjpudWxsLCJuZXh0X2hhc2giOiI3YzQyNmU1OWM1ZGQ0MTkwODJkZGQ0ZWY1Mzk3ZDdhMGM5NmJiYjQxNmVkMWNkOTg3OTMyNDFiYjg5NTY4YjBjIiwiYWNrbm93bGVkZ2VfYmxvY2tjaGFpbl9pZCI6ImFja25vd2xlZGdlLWVscHJveHlzaWduZWQtbXljdXN0b21lcjItaW5maW5pdGUtMCJ9
      --------------------------------------------------------
      Log verification error 1 details:
      - Error code: Log Verification Wrong Hash
      - Error type: Wrong Log Hash
      - Failed iteration_id: 16
      - Expected hash: 34f5bd40d5042ba289d4c5032c75a426306a57e41c0703df4c7698df104f75ed
      - Found hash   : 593bcd654fd80091105a21f548c1e6a8dd07c80380e72ceeeb1a3e7b126d26bb
      - CorruptionId: eyJmcm9tX2l0ZXJhdGlvbiI6MTYsInRvX2l0ZXJhdGlvbiI6MTYsInByZXZpb3VzX2hhc2giOiIwNTE0YmY3YTBmNmRmNmNhMjg1YTIwYTM2OGFiNTA5M2I5NjgxMWZkZWFmMmQ1YThhYjFkOTYwYzgyNDRiNzJlIiwiYWNrbm93bGVkZ2VfYmxvY2tjaGFpbl9pZCI6ImFja25vd2xlZGdlLWVscHJveHlzaWduZWQtbmV0ZXllLW9uZV93ZWVrLTAifQ==
      --------------------------------------------------------
      --------------------------------------------------------

For more details about possible errors that can be reported by the El Proxy verification and
information on the recommended instructions, please review the associated :ref:`errors table <table-elproxy-verification-error-empty-blockchain-corruption>`.

Errors different from the `Empty Blockchain Corruption` error are provided with a *CorruptionId* that uniquely identifies them.
The *CorruptionId* is a base64 encoded JSON that contains data to identify a specific
corruption of a blockchain.

Once the admin has reviewed and investigated the conditions that led to the error, it is possible to fix the
corruptions in one of the following ways:

#. Recover the logs via the :command:`dlq recover` command, in case the corruption was caused by logs that ended up in :ref:`DLQ <dead_letter_queue>`

#. :ref:`Acknowledge the corruptions <acknowledging_blockchain_corruptions>` via the :command:`acknowledge` or :command:`acknowledge-range` commands

.. _acknowledging_blockchain_corruptions:

Acknowledging Blockchain Corruptions
````````````````````````````````````

The :command:`acknowledge` or :command:`acknowledge-range` commands create an acknowledgment for a specific *CorruptionId* so that, when
the :command:`verify` command is executed next time, the linked error in the blockchain will be considered
as resolved.

The acknowledgement data is persisted in a dedicated Elasticsearch index whose name is
generated from the name of the corrupted data stream. For example, if the
corrupted data stream is ``*-*-elproxysigned-neteye-one_week-0``, then the name of the
acknowledged blockchain will be ``acknowledge-elproxysigned-neteye-one_week-0``.

For instance, we can acknowledge the first error reported in the above example by connecting to the
tenant specific container on the DPO machine, as described :ref:`here <elproxy-trigger-manual-verification-container>`,
and executing the following command:

.. code:: bash

   elastic_blockchain_proxy acknowledge \
   --key-file /path/to/secret/key \
   --es-auth-method 'pemcertificatepath' \
   --es-client-cert '/root/elproxy-verification/conf/certs/ElasticBlockchainProxyAcknowledge.crt.pem' \
   --es-client-key '/root/elproxy-verification/conf/certs/private/ElasticBlockchainProxyAcknowledge.key.pem' \
   --corruption-id=eyJmcm9tX2l0ZXJhdGlvbiI6MCwidG9faXRlcmF0aW9uIjoxLCJwcmV2aW91c19oYXNoIjpudWxsLCJuZXh0X2hhc2giOiI3YzQyNmU1OWM1ZGQ0MTkwODJkZGQ0ZWY1Mzk3ZDdhMGM5NmJiYjQxNmVkMWNkOTg3OTMyNDFiYjg5NTY4YjBjIiwiYWNrbm93bGVkZ2VfYmxvY2tjaGFpbl9pZCI6ImFja25vd2xlZGdlLWVscHJveHlzaWduZWQtbXljdXN0b21lcjItaW5maW5pdGUtMCJ9=

The :command:`acknowledge` command is intended to be used for the acknowledgment of a single corruption and it is
not optimized for the acknowledgment of multiple corruptions.
If we want to acknowledge multiple corruptions, we can use the :command:`acknowledge-range` command.

For example, assuming all the corruptions reported in the example above occurred from January 1st 2023 to July 31st 2023 (UTC time)
on the blockchain of tenant  ``mycustomer``, retention ``6_months`` and tag ``0``, we can run, always from the
blockchain specific container on the DPO machine, the following command:

.. code:: bash

    elastic_blockchain_proxy acknowledge-range \
    --tenant 'mycustomer' \
    --retention '6_months' \
    --tag '0' \
    --key-file '/path/to/secret/key' \
    --from '2023-01-01T00:00:00Z' \
    --to '2023-07-31T00:00:00Z' \
    --es-read-auth-method 'pemcertificatepath' \
    --es-read-client-cert '/root/elproxy-verification/conf/certs/neteye_elproxy_verify_mycustomer.crt.pem' \
    --es-read-client-key '/root/elproxy-verification/conf/certs/private/neteye_elproxy_verify_mycustomer.key.pem' \
    --es-write-auth-method 'pemcertificatepath' \
    --es-write-client-cert '/root/elproxy-verification/conf/certs/ElasticBlockchainProxyAcknowledge.crt.pem' \
    --es-write-client-key '/root/elproxy-verification/conf/certs/private/ElasticBlockchainProxyAcknowledge.key.pem'

For more information about the :command:`acknowledge` and :command:`acknowledge-range` commands please
refer to the :ref:`ebp-commands` section.
