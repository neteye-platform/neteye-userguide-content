.. _satayo-rest-api:

SATAYO REST API
~~~~~~~~~~~~~~~

The **SATAYO REST API** allows technical integrations to retrieve and, when
authorized, insert SATAYO findings for their tenant.

Every customer has a dedicated SATAYO instance. In the examples below,
replace ``<satayo-host>`` with the host name of your instance, for example
by exporting it once in your shell:

.. code-block:: bash

    export SATAYO_HOST=<satayo-host>

The API is available at the following base URL:

.. code-block:: text

    https://<satayo-host>/satayo/api/sdk/v0

Authentication
==============

The API uses OAuth 2.0 bearer tokens. Integrations authenticate as a
*service account* using the **client credentials** grant: there is no user
name or password involved, only a client ID and a client secret.

To obtain a client ID and secret for your integration, contact Würth IT
`support <https://servicedesk.wuerth-it.it>`__, specifying which permissions the
integration needs:

* **Read**: retrieve findings of your tenant. This is the standard access level
  for integrations.
* **Import**: insert or update findings. This is granted only on request.

.. warning::

   The client secret grants access to your findings. Store it in a secret
   manager or in an environment variable, never in source code or in a shared
   repository. If it is compromised, contact support to have it rotated.

Step 1: Request an Access Token
-------------------------------

Request a token from the SATAYO identity provider, passing the client ID and
secret you received:

.. code-block:: bash

    export SATAYO_CLIENT_ID=<client-id>
    export SATAYO_CLIENT_SECRET=<client-secret>

    TOKEN=$(curl -s -X POST \
      "https://$SATAYO_HOST/satayo/auth/realms/satayo2/protocol/openid-connect/token" \
      -d grant_type=client_credentials \
      -d client_id="$SATAYO_CLIENT_ID" \
      -d client_secret="$SATAYO_CLIENT_SECRET" \
      | jq -r .access_token)

The response is a JSON object whose ``access_token`` field contains the token,
and whose ``expires_in`` field contains its validity in seconds. When the token
expires, request a new one in the same way.

If the client ID or secret is wrong, the identity provider answers with
``401`` and ``"error": "invalid_client"``.

Step 2 (Optional): Inspect the Token
------------------------------------

The access token is a JSON Web Token (JWT). Its permissions are carried in the
``idp_groups`` claim. To check which groups your client belongs to, decode the
token payload:

.. code-block:: bash

    jq -R 'split(".")[1] | gsub("-";"+") | gsub("_";"/") | @base64d | fromjson
           | {azp, preferred_username, idp_groups}' <<<"$TOKEN"

For a read-only client, the output looks like the following:

.. code-block:: json

    {
      "azp": "<client-id>",
      "preferred_username": "service-account-<client-id>",
      "idp_groups": [
        "/findings-readers"
      ]
    }

Step 3: Call the API
--------------------

Send the token in the ``Authorization`` header of every request. For example,
to retrieve the first page of findings:

.. code-block:: bash

    curl -s -H "Authorization: Bearer $TOKEN" \
      "https://$SATAYO_HOST/satayo/api/sdk/v0/findings/all?limit=1" \
      | jq '{total, page, limit}'

The API returns ``200`` with the pagination information:

.. code-block:: json

    {
      "total": 218,
      "page": 1,
      "limit": 1
    }

A request without a valid token is rejected with ``401``.

Interactive API Documentation
=============================

The complete list of endpoints, parameters and data schemas is available in
the Swagger UI of your instance:

.. code-block:: text

    https://<satayo-host>/satayo/api/swagger/

Select the **SDK** specification to see the endpoints described in this page.
The raw OpenAPI document is available at
``https://<satayo-host>/satayo/api/sdk/v0/openapi.json``.

.. note::

   The Swagger UI and the OpenAPI document require authentication. Log in to
   the SATAYO web interface first, then open the link in the same browser.
   Unauthenticated requests receive ``404``.

While you are logged in, the requests you send from the Swagger UI use your
personal account and its permissions, so you can try the API without a client
ID and secret. For scripts and integrations, always use a service account as
described in `Authentication`_: the API does not issue tokens for personal
accounts.

Common Request Options
======================

* **Pagination**: all list endpoints accept the ``page`` (default ``1``) and
  ``limit`` (default ``100``) query parameters, and return the ``total``
  number of matching findings together with the current ``page`` and
  ``limit``.
* **Closed findings**: only open findings are returned by default. Add
  ``includeclosed=true`` to include closed ones.
* **Tracing**: you can optionally pass a W3C ``traceparent`` header, for
  example ``Traceparent: 00-0af7651916cd43dd8448eb211c80319c-b7ad6b7169203331-01``,
  to correlate your requests with SATAYO logs when contacting support.

Unknown query parameters are rejected with ``422``.

Examples
========

The following examples assume that ``$SATAYO_HOST`` and ``$TOKEN`` are set as
described in :ref:`satayo-rest-api`.

Retrieve Findings of a Given Type
---------------------------------

Each finding type has its own list endpoint, ``/findings/<type>``. For
example, to retrieve credential findings:

.. code-block:: bash

    curl -s -H "Authorization: Bearer $TOKEN" \
      "https://$SATAYO_HOST/satayo/api/sdk/v0/findings/credential?page=1&limit=100"

The API returns ``200`` with a paginated list:

.. code-block:: text

    {
      "total": 218,
      "page": 1,
      "limit": 100,
      "findings": [
        {
          "uid": "Y3JlZGVudGlhbC0zODM5",
          "finding_type": "credential",
          "username": "johndoe@mail.com",
          "resource": "https://www.bank.com/",
          "state": "Unassigned",
          ...
        },
        ...
      ]
    }

The available finding types and the fields returned for each of them are
listed in the Swagger UI.

Retrieve a Single Finding
-------------------------

Use the finding ``uid`` to retrieve its details, including its history and
related findings:

.. code-block:: bash

    curl -s -H "Authorization: Bearer $TOKEN" \
      "https://$SATAYO_HOST/satayo/api/sdk/v0/finding/Y3JlZGVudGlhbC0zODM5"

The API returns ``200`` with an object containing the ``summary`` of the
finding, its ``info``, its ``history`` and its ``related_findings``.

Insert a Finding
----------------

.. note::

   Inserting findings requires a client with the **Import** permission.
   Read-only clients receive ``403``.

Findings are inserted, or updated if they already exist, with ``PUT``
requests. For example, to insert a credential finding for a user identified
by an email address:

.. code-block:: bash

    curl -s -X PUT \
      "https://$SATAYO_HOST/satayo/api/sdk/v0/findings/credentials" \
      -H "Authorization: Bearer $TOKEN" \
      -H "Content-Type: application/json" \
      -d '{
        "monitored_domain_id": 1,
        "tool_code": "toolA",
        "resource_url": "https://www.bank.com",
        "username": "johndoe@mail.com",
        "password": "1234",
        "hash": "03ac674216f3e15c761ee1a5e255f067953623c8b388b4459e13f978d7c846f4",
        "hash_algorithm": "SHA256"
      }'

The fields ``monitored_domain_id``, ``tool_code`` and ``username`` are
required; the others are optional. The API returns ``201`` if the finding was
created, or ``200`` if an existing finding was updated, with the identifier of
the finding in the body:

.. code-block:: json

    {
      "uid": "Y3JlZGVudGlhbC0zODM5"
    }
