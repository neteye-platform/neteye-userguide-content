
SIEM Additional Tuning (X-Pack)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. rubric:: Encrypt sensitive data check

If you use Watcher and have chosen to encrypt sensitive data (by setting
``xpack.watcher.encrypt_sensitive_data`` to **true**), you must also
place a key in the secure settings store.

To pass this bootstrap check, you must set the
``xpack.watcher.encryption_key`` on **each node in the cluster**. For
more information, see the `official
documentation <https://www.elastic.co/guide/en/elasticsearch/reference/7.17/encrypting-data.html>`__.
