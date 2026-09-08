.. _satayo-rest-api:

SATAYO REST API
~~~~~~~~~~~~~~~

The **SATAYO REST API** allows technical integrations to insert, modify, and
retrieve SATAYO findings for their authorized tenant.

Example
=======

As an example, suppose we want to insert a finding about a bank account credential for a
user identified by an email address.

We use the following ``curl`` command to insert it:

.. code-block:: bash

    curl -k -X 'PUT' \
      'https://satayo2.apps-crc.testing/satayo/api/sdk/v0/findings/credentials' \
      -H 'accept: application/json' \
      -H 'Traceparent: 00-0af7651916cd43dd8448eb211c80319c-b7ad6b7169203331-01' \
      -H 'Authorization: Bearer XmiJVCjVLEa2dc8S' \
      -H 'Content-Type: application/json' \
      -d '{
      "monitored_domain_id": 1,
      "tool_code": "toolA",
      "resource_url": "https://www.bank.com",
      "username": "johndoe@mail.com",
      "password": "1234",
      "hash": "03ac674216f3e15c761ee1a5e255f067953623c8b388b4459e13f978d7c846f4",
      "hash_algorithm": "SHA256"
    }'

The API inserts the finding and returns ``201`` with the finding identifier in the body:

.. code-block:: json

    {
      "uid": "Y3JlZGVudGlhbC0zODM5"
    }

Then suppose we want to retrieve that finding. One possible way is to
retrieve the finding using its identifier:

.. code-block:: bash

    curl -k -X 'GET' \
      'https://satayo2.apps-crc.testing/satayo/api/sdk/v0/finding/Y3JlZGVudGlhbC0zODM5' \
      -H 'accept: application/json' \
      -H 'Traceparent: 00-0af7651916cd43dd8448eb211c80319c-b7ad6b7169203331-01' \
      -H 'Authorization: Bearer XmiJVCjVLEa2dc8S'

The API returns ``200`` with the finding details in the body:

.. code-block:: json

    {
      "total": 1,
      "page": 1,
      "limit": 100,
      "findings": [
        {
          "uid": "Y3JlZGVudGlhbC0zODM5",
          "identifier": "1234",
          "finding_type": "credential",
          "tool": {
            "name": "Network Scanner",
            "id": 2
          },
          "organization": {
            "name": "International SummitTrust",
            "id": 1
          },
          "monitored_domain": {
            "name": "international-summittrust.com",
            "id": 1
          },
          "hostname": {
            "data": "www.bank.com",
            "uid": null
          },
          "ipv4_address": null,
          "port": null,
          "severity": null,
          "discovery_date": 1788861915,
          "ticket": null,
          "priority": null,
          "state": "Unassigned",
          "username": "johndoe@mail.com",
          "plain_text": "1234",
          "hash_algorithm": "SHA256",
          "hash": "03ac674216f3e15c761ee1a5e255f067953623c8b388b4459e13f978d7c846f4",
          "email_address": "johndoe@mail.com",
          "resource": "https://www.bank.com/"
        }
      ]
    }

The finding will also appear in the list of credential findings:

.. code-block:: bash

    curl -k -X 'GET' \
      'https://satayo2.apps-crc.testing/satayo/api/sdk/v0/findings/credential' \
      -H 'accept: application/json' \
      -H 'Traceparent: 00-0af7651916cd43dd8448eb211c80319c-b7ad6b7169203331-01' \
      -H 'Authorization: Bearer XmiJVCjVLEa2dc8S'

The API returns ``200`` with the list of findings in the body:

.. code-block:: json

    {
      "total": 218,
      "page": 1,
      "limit": 100,
      "findings": [
        {
          "uid": "Y3JlZGVudGlhbC0zODM5",
          "identifier": "1234",
          "finding_type": "credential",
          "tool": {
            "name": "Network Scanner",
            "id": 2
          },
          "organization": {
            "name": "International SummitTrust",
            "id": 1
          },
          "monitored_domain": {
            "name": "international-summittrust.com",
            "id": 1
          },
          "hostname": {
            "data": "www.bank.com",
            "uid": null
          },
          "ipv4_address": null,
          "port": null,
          "severity": null,
          "discovery_date": 1788861915,
          "ticket": null,
          "priority": null,
          "state": "Unassigned",
          "username": "johndoe@mail.com",
          "plain_text": "1234",
          "hash_algorithm": "SHA256",
          "hash": "03ac674216f3e15c761ee1a5e255f067953623c8b388b4459e13f978d7c846f4",
          "email_address": "johndoe@mail.com",
          "resource": "https://www.bank.com/"
        },
        ...
      ]
    }
