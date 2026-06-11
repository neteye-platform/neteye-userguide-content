.. _tornado-elasticsearch-executor:

Elasticsearch
~~~~~~~~~~~~~

The ELASTICSEARCH Action type allows you to extract data from a Tornado Action and send it to
`Elasticsearch <https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html>`__.

The Elasticsearch Executor behind the Action type expects a Tornado Action to include the following
elements in its payload:

1. **endpoint** : The Elasticsearch endpoint which Tornado will call
   to create the Elasticsearch document (i.e. https://elasticsearch.neteyelocal:9200),

2. **index** : The name of the Elasticsearch index in which the
   document will be created. In the local elasticsearch instance, Tornado can only index data into an index with name ``tornado-*``,

3. **data**: The content of the document that will be sent to Elasticsearch

   .. code:: json

      {
         "user" : "kimchy",
         "post_date" : "2009-11-15T14:12:12",
         "message" : "trying out Elasticsearch"
      }

4. **auth**: Method of authentication; The executor already has a ``default_auth`` configured
   in the file ``/neteye/shared/tornado/conf/elasticsearch_executor.toml``. See more details below.

.. figure:: /core-modules/tornado/img/elasticsearch-action.png

The Elasticsearch Executor will create a new document in the specified
Elasticsearch index for each action executed. In case a specified index
does not yet exist, it will be created by the action.

.. rubric:: Elasticsearch authentication

When the Elasticsearch Action is created, a default authentication
method, ``default_auth``, is defined in the Action's payload and will be used
to authenticate to Elasticsearch.

However, the default method is available only with
the :ref:`|ne| Elastic Stack Feature Module <neteye-modules>` installed.

In case the Feature Module has not been installed, or the default authentication method
is to be overwritten, one should:

- Create a new certificate, signed by signed by the Elasticsearch instance specified in the ``endpoint`` field, or their CA
- Copy the key, certificate and CA to ``/neteye/shared/tornado/conf/certs/``
- Specify the path to the new files in the ``auth`` field

To use a specific authentication method the Action should include the
``auth`` field with either of the following authentication types:
**None** or **PemCertificatePath**.

With **None** authentication type the client connects to Elasticsearch without authentication:

.. code:: json

      {
         "type": "None"
      }


**PemCertificatePath** authentication type means the client connects to
Elasticsearch using the PEM certificates read from the local file system.
When this method is used, the following information must be provided:

   -  **certificate_path**: path to the public certificate accepted by
      Elasticsearch

   -  **private_key_path**: path to the corresponding private key

   -  **ca_certificate_path**: path to CA certificate needed to verify
      the identity of the Elasticsearch server

.. code:: json

      {
         "type": "PemCertificatePath",
         "certificate_path": "/neteye/shared/tornado/conf/certs/acme-elasticsearch.crt.pem",
         "private_key_path": "/neteye/shared/tornado/conf/certs/private/acme-elasticsearch.key.pem",
         "ca_certificate_path": "/neteye/shared/tornado/conf/certs/acme-root-ca.crt"
      }


If a default method is **not** defined upon creation of an Action, then
each action that does not specify authentication method **will
fail**.
