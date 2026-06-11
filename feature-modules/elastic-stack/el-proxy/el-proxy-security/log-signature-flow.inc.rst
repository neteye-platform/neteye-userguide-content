.. _elproxy_generation_of_signature_keys:

Log Signature Flow
~~~~~~~~~~~~~~~~~~

Generation of Signature Keys
````````````````````````````
El Proxy achieves secure logging by authentically encrypting each log record
with an individual cryptographic key, which protects the integrity of the whole
log archive by a cryptographic authentication code. The key is unique
and is used for signing only once.

Each key is bound to a specific customer, module, retention policy and a tag.
A unique combination of values of all mentioned properties serves to define
a single blockchain the incoming log will be added to.

An encryption key for the initial log in a blockchain is generated upon the event
of receiving the log from Logstash to be signed. The key file is saved
on the filesystem in the ``{data_dir}`` folder with the following naming convention:

``{data_dir}/{customer}/{module}-{customer}-{retention_policy}-{blockchain_tag}_key.json``

The key file is expected to contain:

.. code:: json

   {
     "key": "initial_key",
     "iteration": 0,
     "previous_hash": null
   }


where:

- ``key`` is the initial encryption key the initial log is signed with
- ``iteration`` is the iteration number for which the signature key is valid.

As the initial log is signed with the initial key, a new pair key/iteration
is generated based on the latest key used for signature, where

- ``key`` equals to the SHA256 hash of the previous key
- ``iteration`` equals to the previous iteration incremented by one
- ``previous_hash`` is the hash of the last signed log. Is only *null* for
  iteration zero.

For example, if the key at iteration 0 is:

.. code:: json

   {
     "key": "abcdefghilmno",
     "iteration": 0,
     "previous_hash": null
   }

the next key will be:

.. code:: json

   {
     "key": "d1bf...5cb4",
     "iteration": 1,
     "previous_hash": "HASH_OF_THE_PREVIOUS_LOG"
   }

After the initial log is signed, a copy of the key file is created in
the ``{data_backup_dir}`` folder with the following naming convention:

``{data_backup_dir}/{customer}/{module}-{customer}-{retention_policy}-{blockchain_tag}_key.json``

As soon as a new key file appears in the ``{data_backup_dir}`` folder,
the Icinga2 service **logmanager-blockchain-keys-neteyelocal** will enter
in *CRITICAL* state, indicating that a new key has been generated in the system.
The new key must be moved in a safe place such as a password manager or a storage
with restricted access.

This mechanism creates a blockchain of keys that cannot be altered
without breaking the validity of the entire chain.

Every time a set of logs is successfully sent to Elasticsearch, the
corresponding key file will be overwritten atomically with a the new key,
removing the previously used key in the process.

However, in case of unmanageable Elasticsearch errors, the El Proxy
will reply with an error message to Logstash and will reuse the keys of
the failed call for the next incoming logs.

.. note:: To be valid, the ``iteration`` values of signed logs in
   Elasticsearch should be incremental with no missing or duplicated
   values.

When the first log is received after its startup, the El Proxy calls
Elasticsearch to query for the last indexed log ``iteration`` value to
determine the correct ``iteration`` number for the next log. If the
last log ``iteration`` value returned from Elasticsearch is greater
than the value stored in the key file, the El Proxy will fail to
process the log.

Usage of Signature Keys
```````````````````````

For each incoming log, the El Proxy retrieves the first available
encryption key, as described in the previous section, and then uses it
to calculate the `HMAC-SHA256 <https://en.wikipedia.org/wiki/HMAC>`__
hash of the log.

The calculation of the HMAC hash takes into account:

- the log itself as received from Logstash
- the iteration number
- the timestamp
- the hash of the previous log

At this point, the signed log is a simple JSON object composed by the
following fields:

- *All fields of the original log* : all fields from the original log message
- *ES_BLOCKCHAIN*: an object containing all the El Proxy's calculated values. They are:

   - **fields**: fields of the original log used by the signature process
   - **hash**: the hmac hash calculate as described before
   - **previous_hash**: the hmac hash of the previous log message
   - **iteration**: the iteration number of the signature key
   - **timestamp_ms**: the signature epoch timestamp in milliseconds

For example, given this key:

.. code:: json

   {
     "key": "d1bf...5cb4",
     "iteration": 11,
      "previous_hash": "HASH_OF_THE_PREVIOUS_LOG"
   }

when this log is received:

.. code:: json

   {
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
      }
   }

then this signed log will be generated:

.. code:: json

   {
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
   }

The diagram shown in :numref:`figure-elproxy-log-signing` offers a detailed view on how El Proxy
uses the Signature Keys to sign a batch of Logs.

.. _figure-elproxy-log-signing:

.. figure:: /feature-modules/elastic-stack/img/elproxy_log_signing.jpg

   El Proxy Log Signing flowchart

   Below you can find the meaning of the variables used in the flowchart.

   SIG_KEY(b)
      The key used to sign the logs of a given Blockchain **b**.

   ES_LAST_LOG(b)
      The log with greatest iteration saved in Elasticsearch for the Blockchain **b**.


.. _ebp-how-data-stream-name-is-determined:

How the data stream name is determined
``````````````````````````````````````

The name of the Elasticsearch `data stream <https://www.elastic.co/guide/en/elasticsearch/reference/current/data-streams.html>`__
for the signed logs is determined by the content of the ``EBP_METADATA`` field of the incoming Log.

The data stream name has the following structure:

``{EBP_METADATA.agent.type}-{EBP_METADATA.agent.version}-{EBP_METADATA.event.module}-{EBP_METADATA.customer}-{EBP_METADATA.retention}-{EBP_METADATA.blockchain_tag}``

The following rules and constraints are valid:

- All of these fields are mandatory:

    - ``EBP_METADATA.agent.type``
    - ``EBP_METADATA.customer``
    - ``EBP_METADATA.retention``
    - ``EBP_METADATA.blockchain_tag``

- If the ``{EBP_METADATA.event.module}`` is not present, El Proxy will use by default ``elproxysigned``

- If the fields ``{EBP_METADATA.data_stream.type}`` and ``{EBP_METADATA.data_stream.dataset}`` are present,
  they are used instead of ``{EBP_METADATA.agent.type}`` and ``{EBP_METADATA.agent.version}`` respectively
  in the data stream name.

For example, given this log is received on 23 March, 2021:

.. code:: json

   {
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
      }
   }

Then the inferred data stream name is: ``auditbeat-7.10.1-elproxysigned-neteye-6_months-0``

As a consequence of the default values and of the default Logstash configuration,
most of the data streams created by El Proxy will have ``elproxysigned`` in the name.
Consequently, special care should be applied when manipulating those data streams and documents;
in particular, the user must not delete or rename ``*-elproxysigned-*`` data streams manually nor alter
the content of ``ES_BLOCKCHAIN`` or ``EBP_METADATA`` fields as any change could lead to a broken
blockchain.

How Elasticsearch Ingest Pipeline are defined for each log
``````````````````````````````````````````````````````````

A common use case is that logs going through El Proxy need to be enriched by some
`Elasticsearch Ingest Pipeline <https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest.html>`__
when they are indexed in Elasticsearch.

El Proxy supports this use case and allows its callers to specify, for each log, the ID
of Ingest Pipeline that needs to enrich the log. The ID of the Ingest Pipeline is
defined by the field ``EBP_METADATA.pipeline`` of the incoming log.
If ``EBP_METADATA.pipeline`` is left empty, the log will not be preprocessed by any
specific Ingest Pipeline.
