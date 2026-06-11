.. _el-proxy-config-files:

Configuration files
~~~~~~~~~~~~~~~~~~~

Additional configuration entries are
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
