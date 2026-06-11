.. _ebp-commands:

El Proxy Commands
~~~~~~~~~~~~~~~~~

El Proxy is carried out with the **CLI commands**
that allow you to use the functionality provided.  Running the
executable without any arguments returns a list of all available
commands and global options that apply to every command.

Available commands:

- **acknowledge** : Acknowledges a corruption in a blockchain.
- **acknowledge-range** : Acknowledges all the corruptions in a blockchain given a time range.
- **dlq recover** : Recovers signed logs inside the Dead Letter Queue.
- **export-logs** : Exports the signed logs from ElasticSearch to a local file.
- **serve** : Starts El Proxy server ready for processing incoming requests.
- **verify** : Verify the validity of a blockchain.
- **verify-cpu-bench** : Benchmark the underlying hardware for verification.

El Proxy configuration is partly based on
configuration files and partly based on command line parameters. The
location of configuration files in the file system is determined at
startup based on the provided CLI options.
In addition, each command can have specific CLI arguments required.

Global command line parameters:

- **config-dir**: (Optional) The filesystem folder from which the
  configuration is read. The default path is
  :file:`/neteye/shared/elastic-blockchain-proxy/conf/`.
- **static-config-dir**: (Optional) The filesystem folder from which the
  static configuration (i.e. configuration files that the final user cannot modify)
  is read. The default path is
  :file:`/usr/share/elastic-blockchain-proxy`.

Below you will find available command line parameters for all El Proxy commands.

.. rubric:: **acknowledge** command

- **key-file**: The path to the file that contains the iteration 0 signature key
- **corruption-id**: The CorruptionId produced by the *verify* command to be acknowledged
- **description**: (Optional) A description of the blockchain corruption. It will be prompted
  if not provided
- **reason**: (Optional) The reason why the corruption happened. It will be prompted
  if not provided
- **es-auth-method**: (Optional) The method used to authenticate
  to Elasticsearch. This can be:

  - **none**: (Default) the command does not authenticate to Elasticsearch
  - **basicauth**: Username and password are used to authenticate. If this method is specified,
    the following parameter is required (and a password will be prompted during the execution):

    - **es-user**: the name Elasticsearch user used to perform authentication

  - **pemcertificatepath**: PKI user authentication is used. If this method is specified,
    the following parameters are required:

    - **es-client-cert** path to the client certificate.
      A client certificate suitable for the acknowledge command is present, in each container run on the DPO machine, under
      :file:`/root/elproxy-verification/conf/certs/ElasticBlockchainProxyAcknowledge.crt.pem`
    - **es-client-key** path to the private key of the client certificate.
      A client private key suitable for the acknowledge command is present, in each container run on the DPO machine, under
      :file:`/root/elproxy-verification/conf/certs/private/ElasticBlockchainProxyAcknowledge.key.pem`
    - **es-ca-cert** path to the CA certificate to be trusted during the requests
      to Elasticsearch

.. note:: The following parameters are now deprecated:

   - **elasticsearch-authentication-method**: Use **es-auth-method** instead
   - **elasticsearch-username**: Use **es-user** instead
   - **elasticsearch-client-cert**: Use **es-client-cert** instead
   - **elasticsearch-client-private-key**: Use **es-client-key** instead
   - **elasticsearch-ca-cert**: Use **es-ca-cert** instead

For example, if you want to acknowledge the corruption with corruption-id `abc123`,
you can execute the following command:

.. code:: bash

    elastic_blockchain_proxy acknowledge \
    --key-file '/path/to/secret/key' \
    --corruption-id 'abc123' \
    --es-auth-method 'pemcertificatepath' \
    --es-client-cert '/root/elproxy-verification/conf/certs/ElasticBlockchainProxyAcknowledge.crt.pem' \
    --es-client-key '/root/elproxy-verification/conf/certs/private/ElasticBlockchainProxyAcknowledge.key.pem'

.. note:: This command should be executed from one of the tenant-specific containers of the DPO machine.
   Furthermore, the command is intended to be used for the acknowledgment of one corruption at the time and it is
   not optimized for the acknowledgdment of multiple corruptions. If you want to acknowledge multiple corruptions,
   please refer to the :command:`acknowledge-range` command.

.. rubric:: **acknowledge-range** command

- **tenant**: Tenant of the blockchain
- **retention**: The retention name of the blockchain
- **tag**: The tag of the blockchain
- **key-file**: The path to the file that contains the iteration 0 signature key
- **from**: The inclusive date in RFC3339 format from which the corruptions should be acknowledged

  .. note:: In case the provided **from** timestamp does not match the timestamp of an actual log, the first log
     prior to that timestamp will be considered as a starting point for the :command:`acknowledge-range` command,
     to ensure we correctly recognise and acknowledge the cases in which the **from** timestamp
     appears in a hole of the blockchain, so in correspondence of some missing logs

- **to**: The exclusive date in RFC3339 format until which the corruptions should be acknowledged
- **batch-size**: (Optional) The size of each read/verify operation. Default is 500.
- **concurrent-batches**: (Optional) The number of concurrent threads verifying the blockchain, to discover the corruptions to acknowledge. Default is 2.
- **description**: (Optional) A description of the blockchain corruption, which will be associated with all acknowledged corruptions. It will be prompted
  if not provided
- **reason**: (Optional) The reason why the corruption happened, which will be associated with all acknowledged corruptions. It will be prompted
  if not provided
- **es-read-auth-method**: (Optional) The method used to authenticate
  to Elasticsearch in order to read from it. This can be:

  - **none**: (Default) the command does not authenticate to Elasticsearch
  - **basicauth**: Username and password are used to authenticate. If this method is specified,
    the following parameter is required (and a password will be prompted during the execution):

    - **es-read-user**: the name Elasticsearch user with read permissions used to perform authentication

  - **pemcertificatepath**: PKI user authentication is used. If this method is specified,
    the following parameters are required:

    - **es-read-client-cert** path to the client certificate with read permissions
    - **es-read-client-key** path to the private key of the client certificate with read permissions
    - **es-read-ca-cert** path to the CA certificate to be trusted during the requests
      to Elasticsearch

- **es-write-auth-method**: (Optional) The method used to authenticate
  to Elasticsearch in order to write to it. This can be:

  - **none**: (Default) the command does not authenticate to Elasticsearch
  - **basicauth**: Username and password are used to authenticate. If this method is specified,
    the following parameter is required (and a password will be prompted during the execution):

    - **es-write-user**: the name Elasticsearch user with write permissions used to perform authentication

  - **pemcertificatepath**: PKI user authentication is used. If this method is specified,
    the following parameters are required:

    - **es-write-client-cert** path to the client certificate with write permissions
    - **es-write-client-key** path to the private key of the client certificate with write permissions
    - **es-write-ca-cert** path to the CA certificate to be trusted during the requests
      to Elasticsearch

.. note:: The following parameters are now deprecated:

   - **index-name**: Use **tenant**, **retention** and **tag** instead.

For example, if you want to acknowledge all the corruptions from January 1st 2023 to July 31st 2023 (UTC time)
on the index pattern ``*-*-elproxysigned-mytenant-6_months-0``,
you can execute the following command:


.. code:: bash

    elastic_blockchain_proxy acknowledge-range \
    --tenant 'mytenant' \
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

In case your infrastructure is set to Italian timezone (daylight saving time, so UTC+2) and you would like to use this format when
setting the parameters for range acknowledging, you can execute the following command:

.. code:: bash

    elastic_blockchain_proxy acknowledge-range \
    --tenant 'mytenant' \
    --retention '6_months' \
    --tag '0' \
    --key-file '/path/to/secret/key' \
    --from '2023-01-01T00:00:00+02:00' \
    --to '2023-07-31T00:00:00+02:00' \
    --es-read-auth-method 'pemcertificatepath' \
    --es-read-client-cert '/root/elproxy-verification/conf/certs/neteye_elproxy_verify_mycustomer.crt.pem' \
    --es-read-client-key '/root/elproxy-verification/conf/certs/private/neteye_elproxy_verify_mycustomer.key.pem' \
    --es-write-auth-method 'pemcertificatepath' \
    --es-write-client-cert '/root/elproxy-verification/conf/certs/ElasticBlockchainProxyAcknowledge.crt.pem' \
    --es-write-client-key '/root/elproxy-verification/conf/certs/private/ElasticBlockchainProxyAcknowledge.key.pem'

.. note:: This command should be executed from one of the tenant-specific containers of the DPO machine.

.. rubric:: **dlq recover** command

- **dlq-dir**: (Optional) The path to the Dead Letter Queue folder. The path defaults to the **dlq_dir** entry
  of the configuration file :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.toml`.
  The :command:`dlq recover` command expects a specific file structure inside the **dlq-dir** folder.
  The **dlq-dir** folder should only contain subfolders and the subfolders should only contain
  :abbr:`NDJSON (Newline Delimited JSON)` files with specific names.
- **dlq-recovered-dir**: (Optional) The path to the folder where the successfully
  recovered logs will be stored. The path defaults to the **dlq_recovered_dir** entry
  of the configuration file :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.toml`.
- **batch-size**: (Optional) The size of each read/write operation. Default is 500.
- **es-auth-method**: (Optional) The method used to authenticate
  to Elasticsearch. This can be:

  - **none**: (Default) the command does not authenticate to Elasticsearch
  - **basicauth**: Username and password are used to authenticate. If this method is specified,
    the following parameter is required (and a password will be prompted during the execution):

    - **es-user**: the name Elasticsearch user used to perform authentication

  - **pemcertificatepath**: PKI user authentication is used. If this method is specified,
    the following parameters are required:

    - **es-client-cert** path to the client certificate.
      A client certificate suitable for the :command:`dlq recover` command is present under
      :file:`/neteye/shared/elastic-blockchain-proxy/conf/certs/ElasticBlockchainProxyRecover.crt.pem`
    - **es-client-key** path to the private key of the client certificate.
      A client private key suitable for the :command:`dlq recover` command is present under
      :file:`/neteye/shared/elastic-blockchain-proxy/conf/certs/private/ElasticBlockchainProxyRecover.key.pem`
    - **es-ca-cert** path to the CA certificate to be trusted during the requests
      to Elasticsearch

.. note:: The following parameters are now deprecated:

   - **elasticsearch-authentication-method**: Use **es-auth-method** instead
   - **elasticsearch-username**: Use **es-user** instead
   - **elasticsearch-client-cert**: Use **es-client-cert** instead
   - **elasticsearch-client-private-key**: Use **es-client-key** instead
   - **elasticsearch-ca-cert**: Use **es-ca-cert** instead

In the most common scenarios you can run the following command, which will recover the
DLQ logs that are present under the default directory where El Proxy stores DLQ logs:

.. code:: bash

    elastic_blockchain_proxy dlq recover \
    --es-auth-method 'pemcertificatepath' \
    --es-client-cert '/neteye/shared/elastic-blockchain-proxy/conf/certs/ElasticBlockchainProxyRecover.crt.pem' \
    --es-client-key '/neteye/shared/elastic-blockchain-proxy/conf/certs/private/ElasticBlockchainProxyRecover.key.pem'

If instead you want to recover DLQ logs stored under a custom directory :file:`/my/custom/dlq-dir`
and let the successfully recovered logs be stored under :file:`/my/custom/dlq-recovered-dir`,
you can execute the following command:

.. code:: bash

    elastic_blockchain_proxy dlq recover \
    --dlq-dir '/my/custom/dlq-dir'
    --dlq-recovered-dir '/my/custom/dlq-recovered-dir'
    --es-auth-method 'pemcertificatepath' \
    --es-client-cert '/neteye/shared/elastic-blockchain-proxy/conf/certs/ElasticBlockchainProxyRecover.crt.pem' \
    --es-client-key '/neteye/shared/elastic-blockchain-proxy/conf/certs/private/ElasticBlockchainProxyRecover.key.pem'

.. rubric:: **export-logs** command

- **output-file**: The output file path where the exported logs are written
- **tenant**: Tenant of the blockchain
- **retention**: The retention name of the blockchain
- **tag**: The tag of the blockchain
- **format**: (Optional) The output file format. This can be:

  - **ndjson**: (Default) the logs are exported in `Newline delimited JSON <http://ndjson.org/>`__ format

- **from**: (Optional) An inclusive 'From' date limit in RFC3339 format
- **to**: (Optional) An exclusive 'To' date limit in RFC3339 format
- **batch-size**: (Optional) The size of a each read/write operation. Default is 500.
- **es-auth-method**: (Optional) The method used to authenticate
  to Elasticsearch. This can be:

  - **none**: (Default) the command does not authenticate to Elasticsearch
  - **basicauth**: Username and password are used to authenticate. If this method is specified,
    the following parameter is required (and a password will be prompted during the execution):

    - **es-user**: the name Elasticsearch user used to perform authentication

  - **pemcertificatepath**: PKI user authentication is used. If this method is specified,
    the following parameters are required:

    - **es-client-cert** path to the client certificate
    - **es-client-key** path to the private key of the client certificate
    - **es-ca-cert** path to the CA certificate to be trusted during the requests
      to Elasticsearch

.. note:: The following parameters are now deprecated:

   - **from**: Use **from-date** instead
   - **to**: Use **to-date** instead
   - **elasticsearch-authentication-method**: Use **es-auth-method** instead
   - **elasticsearch-username**: Use **es-user** instead
   - **elasticsearch-client-cert**: Use **es-client-cert** instead
   - **elasticsearch-client-private-key**: Use **es-client-key** instead
   - **elasticsearch-ca-cert**: Use **es-ca-cert** instead
   - **index-name**: Use **tenant**, **retention** and **tag** instead.


.. rubric:: **verify** command

- **tenant**: Tenant of the blockchain
- **retention**: The retention name of the blockchain
- **tag**: The tag of the blockchain
- **key-file**: The path to the file that contains the iteration 0 signature key
- **batch-size**: (Optional) The size of each read/verify operation. Default is 500.
- **concurrent-batches**: (Optional) The number of concurrent threads verifying the blockchain. Default is 2.
- **skip-iterations-before**: (Optional) The inclusive iteration number from which the verification has
  to be performed. Default is 0.

    .. note:: It is not recommended to use this parameter when verifying the blockchain in production as it is intended for testing purposes only.

  We highlight that this parameter is **retention-aware**, which means it will be
  considered only if the specified iteration number is present, when performing the verification, given the set retention.
  Otherwise, in case for example it refers to an older iteration number already deleted by Elasticsearch
  when applying the retention, the specified iteration will be ignored.
- **es-auth-method**: (Optional) The method used to authenticate
  to Elasticsearch. This can be:

  - **none**: (Default) the command does not authenticate to Elasticsearch
  - **basicauth**: Username and password are used to authenticate. If this method is specified,
    the following parameter is required (and a password will be prompted during the execution):

    - **es-user**: the name Elasticsearch user used to perform authentication

  - **pemcertificatepath**: PKI user authentication is used. If this method is specified,
    the following parameters are required:

    - **es-client-cert** path to the client certificate
    - **es-client-key** path to the private key of the client certificate
    - **es-ca-cert** path to the CA certificate to be trusted during the requests
      to Elasticsearch
- **elasticsearch-indexing-delay**: (Optional) The expected maximum time in seconds
  spent by Elasticsearch to index a document. Useful when verifying recently created logs that could need
  some time to be indexed by Elasticsearch. Default is *60* seconds.

.. note:: The following parameters are now deprecated:

   - **elasticsearch-authentication-method**: Use **es-auth-method** instead
   - **elasticsearch-username**: Use **es-user** instead
   - **elasticsearch-client-cert**: Use **es-client-cert** instead
   - **elasticsearch-client-private-key**: Use **es-client-key** instead
   - **elasticsearch-ca-cert**: Use **es-ca-cert** instead
   - **from-iteration**: Use **skip-iterations-before** instead.
   - **index-name**: Use **tenant**, **retention** and **tag** instead.

For example, given a blockchain which refers to:

* `module`: ``elproxysigned``
* `tenant`: ``mytenant``
* `retention`: ``6_months``
* `blockchain_tag`: ``0``

You can verify the blockchain with the command:

.. code:: bash

    elastic_blockchain_proxy verify \
    --tenant 'mytenant' \
    --retention '6_months' \
    --tag '0' \
    --key-file '/path/to/secret/key' \
    --es-auth-method 'pemcertificatepath' \
    --es-client-cert '/root/elproxy-verification/conf/certs/neteye_elproxy_verify_mycustomer.crt.pem' \
    --es-client-key '/root/elproxy-verification/conf/certs/private/neteye_elproxy_verify_mycustomer.key.pem'

.. note:: This command should be executed from one of the tenant-specific containers of the DPO machine.


.. rubric:: **verify-cpu-bench** command

- **batch-size**: (Optional) The size of each read/verify operation. Default is 1000.
- **n-logs**: (Optional) The number of logs to verify. Default is 1,000,000.
- **log-size**: (Optional) The size of each log in KB. Default is 1KB.
- **concurrent-batches**: (Optional) The number of concurrent threads for the benchmark. Default is 2.

You can check, how much data throughput your server can handle, based on the number of threads with the following command:

.. code:: bash

    elastic_blockchain_proxy verify-cpu-bench

Besides these parameters, additional configuration entries are
available in the following files:

- :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.toml`
- :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy_fields.toml`

To customize the available options, described below, one or more file must be created
in the :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.d` folder, specifying on each line
an option and its value in the ``OPTION = VALUE`` format, e.g. ``message_queue_size = 20000``.

The :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.toml` file contains
the following configuration entries:

-  **logger**:

   -  **level**: The Logger level filter; valid values are *trace*,
      *debug*, *info*, *warn*, and *error*. The logger level filter can
      specify a list of comma separated per-module specific levels, for
      example: *warn,elastic_blockchain_proxy=debug*

-  **failure_max_retries**: A predefined maximum amount of retry
   attempts for writing logs to Elasticsearch and for writing DLQ files.
   A value of *0* means that no retries will be attempted.
-  **failure_sleep_ms_between_retries**: A fixed amount of milliseconds
   to sleep between each retry attempt.
-  **data_dir**: The path to the folder that contains the ``key.json``
   file. If the file is not present, El Proxy will generate one when needed.
-  **data_backup_dir**: The path where El Proxy creates a backup copy of
   the automatically generated keys. for security reasons, the user is
   in charge of moving these copies to a protected place as soon as possible.
-  **dlq_dir**: The path where the Dead Letter Queue with the failed logs is saved.
-  **dlq_recovered_dir**: The path where the successfully recovered logs are saved
   after running the :command:`dlq recover` command.
-  **blockchain_state_history_dir**: path to the folder that contains the Blockchain State History
   :abbr:`BSH (Blockchain State History)` files used by the verification process.
-  **verification_reports_dir**: The path where the verification reports are stored.
-  **message_queue_size**: The size of the in-memory queue where
   messages will be stored before while waiting for being processed
-  **web_server**:

   -  **address**: The address where El Proxy
      Web Server will listen for HTTP requests
   -  **tls**: TLS options to enable HTTPS
      (See the ``Enabling TLS`` section below)

-  **elasticsearch**:

   -  **url**: The URL of the ElasticSearch server
   -  **timeout_secs**: the timeout in seconds for a
      connection to Elasticsearch
   -  **ca_certificate_path**: path to CA certificate needed to verify the identity of the Elasticsearch server
   -  **auth**: The authentication method to connect to the ElasticSearch server
      (See the ``Elasticsearch authentication`` section below)


The :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy_fields.toml` file contains
the following configuration entry:

-  **include_fields**: List of fields of the log that will be included in the signature process.
   Every field not included in this list will be ignored.
   The dot symbol is used as expander processor; for example, the field
   "name1.name2" refers to the "name2" nested field inside "name1".
