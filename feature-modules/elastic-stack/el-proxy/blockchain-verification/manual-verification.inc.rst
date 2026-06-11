.. _ebp-manual-blockchain-verification:

Manual Blockchain Verification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section aims to define the guidelines for the manual verification of a blockchain.
The following naming convention is used while defining the guidelines for manual verification:

- **<tenant>**: The tenant (also referred to as "customer" in the previous chapters) of the blockchain
  that needs to be verified

- **<retention>**: the retention of the blockchain to be verified

- **<tag>**: the tag of the blockchain to be verified

.. note:: While following this guide and executing commands, remember to always substitute
   the placeholders **<...>** with their real value.

The guide is based on the two examples which should cover most of the use cases.

For both examples, we assume the following:

#. El Proxy has been correctly set up by following the
   the :ref:`NetEye Setup <ebp-verify-neteye-setup>` section

#. Every month has exactly 30 days (to facilitate the date calculations)

Manual verification with existing :abbr:`BSH (Blockchain State History)` file
    In this example, we assume the following:

    #. The current date is ``2022-08-30``

    #. Stored on the |ne| machine, there is a non-empty blockchain created on ``2022-05-01`` with
       the following properties:

        #. ``TENANT``: ``exampletenant``

        #. ``RETENTION``: ``3_months``

        #. ``TAG``: ``exampletag``

    #. The :abbr:`BSH (Blockchain State History)` file is stored under its default path
       on the DPO machine and contains 90 entries (one entry for each day) from ``2022-05-01`` to ``2022-07-30``

    Note that, on the current date, the blockchain is already missing some logs. Since the retention policy
    is three months and the blockchain was created on ``2022-05-01``, the logs indexed from
    ``2022-05-01`` to ``2022-05-30`` have been automatically deleted by Elasticsearch.

    In order to start the verification manually, the DPO admin needs to connect to the running verification container,
    see :ref:`elproxy-trigger-manual-verification-container`, and then run:

    .. code:: bash

       /usr/bin/elastic_blockchain_proxy verify \
       --key-file "/root/elproxy-verification/data/keys/elproxysigned-exampletenant-3_months-exampletag_key.json" \
       --tenant "exampletenant" \
       --retention "3_months" \
       --tag "exampletag" \
       --elasticsearch-authentication-method pemcertificatepath \
       --elasticsearch-client-cert /root/elproxy-verification/conf/certs/neteye_ebp_verify_exampletenant.crt.pem \
       --elasticsearch-client-private-key /root/elproxy-verification/conf/certs/private/neteye_ebp_verify_exampletenant.key.pem

    Internally, the :command:`verify` command tries to retrieve the iteration from which to start the verification;
    based on the current date and the retention policy of the blockchain, El Proxy expects the first iteration of
    the blockchain to be the first log indexed on ``2022-06-01``.

    This date was calculated as follows:

    .. code::

        date = current_date - retention_policy_in_days + 1

    Where ``date`` is the resulting date, ``current_date`` is the current date, and ``retention_policy_in_days`` is the
    retention policy in days of the blockchain. For completeness, the final ``+ 1``
    avoids the resulting date being the date on which Elasticsearch is applying the retention policy.

    To retrieve the actual first iteration from which to start the verification, El Proxy fetches the
    :abbr:`BSH (Blockchain State History)` file searching for the entry corresponding to ``date``.
    As an example, a section of the :abbr:`BSH (Blockchain State History)` file could
    look like the following one:

    .. code:: json

        "2022-05-30": {
            "first_iteration": 15
            "last_iteration": 20
        },
        "2022-06-01": {
            "first_iteration": 21
            "last_iteration": 23
        },
        "2022-06-02": {
            "first_iteration": 24
            "last_iteration": 30
        }

    In this case, the first iteration from which to start the verification is iteration ``21``. At this point,
    El Proxy can start verifying the logs from the retrieved first iteration. If the verification is successful,
    the old :abbr:`BSH (Blockchain State History)` file will be overwritten with a new one
    containing one entry for each day from ``2022-06-01`` to the current date.

Manual verification of newly created blockchain
    In this example, we suppose the following:

    #. The current date is ``2022-09-01``

    #. Stored on the |ne| machine, there is a non-empty blockchain created on the current date with
       the following properties:

        #. ``TENANT``: ``exampletenant``

        #. ``RETENTION``: ``3_months``

        #. ``TAG``: ``exampletag``

    #. The :abbr:`BSH (Blockchain State History)` file does not exist

    To start the verification manually, the DPO admin needs to connect to the running verification container,
    see :ref:`elproxy-trigger-manual-verification-container`, and then run:

    .. code:: bash

       /usr/bin/elastic_blockchain_proxy verify \
       --key-file "/root/elastic-blockchain-proxy/data/keys/elproxysigned-exampletenant-3_months-exampletag_key.json" \
       --tenant "exampletenant" \
       --retention "3_months" \
       --tag "exampletag" \
       --elasticsearch-authentication-method pemcertificatepath \
       --elasticsearch-client-cert /root/elproxy-verification/conf/certs/neteye_ebp_verify_exampletenant.crt.pem \
       --elasticsearch-client-private-key /root/elproxy-verification/conf/certs/private/neteye_ebp_verify_exampletenant.key.pem

    Internally, the :command:`verify` command tries to retrieve the iteration from which to start the verification.
    Since the :abbr:`BSH (Blockchain State History)` file is missing, El Proxy tries to get the
    first existing log by querying Elasticsearch.

    In this example, since the blockchain is new and all its logs are still present, Elasticsearch
    will return the log with iteration zero as the first log in the blockchain.

    El Proxy will then start the verification from the log with iteration zero. If the verification succeeds,
    a :abbr:`BSH (Blockchain State History)` file will be created and stored on the DPO machine.
