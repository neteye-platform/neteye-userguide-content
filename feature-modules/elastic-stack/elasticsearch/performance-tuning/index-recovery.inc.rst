
Elasticsearch Index Recovery Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

During the process of index recovery on a NetEye cluster, default settings of Elasticsearch
may not appear optimal.

The default limit is set to the following values:

.. code:: json

   {
       "max_concurrent_operations": "1",
       "max_bytes_per_sec": "40mb"
   }

which means that the reallocation of indices may appear to be slow when node leaves
the Elasticsearch cluster. Default settings may under-utilize the internal cluster network bandwidth,
so it is recommended to update the settings dynamically using the cluster update settings API call:

.. code:: bash

    /usr/share/neteye/elasticsearch/scripts/es_curl.sh -XPUT -H 'content-type: application/json' https://elasticsearch.neteyelocal:9200/_cluster/settings -d '{
      "persistent" : {
        "indices.recovery.max_bytes_per_sec" : "100mb",
        "indices.recovery.max_concurrent_operations": 2
      }
    }'

This dynamic setting will apply the same limit on every node in the Cluster.

For a |ne| Elastic Stack module it is required to have a 10GB/s Private connection for the Cluster,
with all the nodes having the same capabilities.

When updating the settings, the ``max_bytes_per_sec`` value should be set to max 50% of the private network bandwidth
if Operative nodes are also Elastic Data. In case Operative Nodes are not Data Nodes
(i.e. all the data are stored on Elastic Data-only Nodes) the value can go up to 95% of the bandwidth.

The ``max_concurrent_operations`` value is recommended to be set to 2, and can be increased (e.g. 4) for larger clusters.

You can find more details in the `official documentation <https://www.elastic.co/guide/en/elasticsearch/reference/current/recovery.html#recovery-settings>`__.
