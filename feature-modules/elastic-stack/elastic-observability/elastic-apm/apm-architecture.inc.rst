
.. _apm-architecture:

Elastic APM Architecture
~~~~~~~~~~~~~~~~~~~~~~~~

The key component of Elastic APM is the **APM Server**, which receives
performance data from applications, converts the data into Elasticsearch
documents and saves these documents in Elasticsearch.

Applications can send performance data to the APM Server, hosted in the
:ref:`Elastic Agent<elastic-agent>`, via the
`APM Agents <https://www.elastic.co/guide/en/apm/agent/index.html>`__.
Some NetEye applications, like Tornado or :ref:`Alyvix <visualize_alyvix_performance_metrics_in_elastic>`,
can already send performance data to the APM Server, while other applications need first to be
integrated with the APM Agents.
