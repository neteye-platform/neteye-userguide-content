.. _tornado-backend:

Tornado Backend API v1
----------------------

The Tornado Backend contains endpoints that allow you to interact with
Tornado through
`REST <https://en.wikipedia.org/wiki/Representational_state_transfer>`__
endpoints.

In this section we describe the version 1 of the Tornado Backend APIs.

.. _auth-backend-api:

Tornado 'Auth' Backend API
``````````````````````````

The 'auth' APIs require the caller to pass an authorization token in the
headers in the format:

``Authorization : Bearer TOKEN_HERE``

The token should be a base64 encoded JSON with this user data:

.. code:: json

   {
     "user": "THE_USER_IDENTIFIER",
     "roles": ["ROLE_1", "ROLE_2", "ROLE_2"]
   }

In the coming releases the current token format will be replaced by a
`JSON Web Token (JWT) <https://en.wikipedia.org/wiki/JSON_Web_Token>`__.

Tornado 'Config' Backend API
````````````````````````````

The 'config' APIs require the caller to pass an authorization token in
the headers as in the 'auth' API.

.. rubric:: Working with configuration and drafts

These endpoints allow working with the configuration and the drafts

Endpoint: get the current Tornado configuration

-  HTTP Method: **GET**
-  path : **/api/v1_beta/config/current**
-  response type: **JSON**
-  response example:

   .. code:: json

       {
         "type": "Rules",
         "rules": [
           {
             "name": "all_emails",
             "description": "This matches all emails",
             "continue": true,
             "active": true,
             "constraint": {
               "WHERE": {
                 "type": "AND",
                 "operators": [
                   {
                     "type": "equal",
                     "first": "${event.type}",
                     "second": "email"
                   }
                 ]
               },
               "WITH": {}
             },
             "actions": [
               {
                 "id": "Logger",
                 "payload": {
                   "subject": "${event.payload.subject}",
                   "type": "${event.type}"
                 }
               }
             ]
           }
         ]
       }

Endpoint: get list of draft ids

-  HTTP Method: **GET**
-  path : **/api/v1_beta/config/drafts**
-  response type: **JSON**
-  response: An array of *String* ids
-  response example:

   .. code:: json

      ["id1", "id2"]

Endpoint: get a draft by id

-  HTTP Method: **GET**
-  path : **/api/v1_beta/config/drafts/{draft_id}**
-  response type: **JSON**
-  response: the draft content
-  response example:

   .. code:: json

       {
         "type": "Rules",
         "rules": [
           {
             "name": "all_emails",
             "description": "This matches all emails",
             "continue": true,
             "active": true,
             "constraint": {
               "WHERE": {},
               "WITH": {}
             },
             "actions": []
           }
         ]
       }

Endpoint: create a new draft and return the draft id. The new draft is
an exact copy of the current configuration; anyway, a root Filter node
is added if not present.

-  HTTP Method: **POST**
-  path : **/api/v1_beta/config/drafts**
-  response type: **JSON**
-  response: the draft content
-  response example:

   .. code:: json

      {
        "id": "id3"
      }

Endpoint: update an existing draft

-  HTTP Method: **PUT**
-  path : **/api/v1_beta/config/drafts/{draft_id}**
-  request body type: **JSON**
-  request body: The draft content in the same JSON format returned by
   the **GET** **/api/v1_beta/config/drafts/{draft_id}** endpoint
-  response type: **JSON**
-  response: an empty json object

Endpoint: delete an existing draft

-  HTTP Method: **DELETE**
-  path : **/api/v1_beta/config/drafts/{draft_id}**
-  response type: **JSON**
-  response: an empty json object

Endpoint: take over an existing draft

-  HTTP Method: **POST**
-  path : **/api/v1_beta/config/drafts/{draft_id}/take_over**
-  response type: **JSON**
-  response: an empty json object

Endpoint: deploy an existing draft

-  HTTP Method: **POST**
-  path : **/api/v1_beta/config/drafts/{draft_id}/deploy**
-  response type: **JSON**
-  response: an empty json object

Tornado 'Event' Backend API
```````````````````````````

.. rubric:: Send Test Event Endpoint

Endpoint: match an event on the current Tornado Engine configuration

-  HTTP Method: **POST**

-  path : **/api/v1_beta/event/current/send**

-  request type: **JSON**

-  request example:

   .. code:: json

      {
          "event": {
            "type": "the_event_type",
            "created_ms": 123456,
            "payload": {
              "value_one": "something",
              "value_two": "something_else"
            }
          },
          "process_type": "SkipActions"
      }

   Where the event has the following structure:

   -  **type**: The Event type identifier
   -  **created_ms**: The Event creation timestamp in milliseconds since
      January 1, 1970 UTC
   -  **payload**: A Map<String, Value> with event-specific data
   -  **process_type**: Can be *Full* or *SkipActions*:

      -  *Full*: The event is processed and linked actions are executed
      -  *SkipActions*: The event is processed but actions are not
         executed

-  response type: **JSON**

-  response example:

   .. code:: json

      {
       "event": {
         "type": "the_event_type",
         "created_ms": 123456,
         "payload": {
           "value_one": "something",
           "value_two": "something_else"
         }
       },
       "result": {
         "type": "Rules",
         "rules": {
           "rules": {
             "emails_with_temperature": {
               "rule_name": "emails",
               "status": "NotMatched",
               "actions": [],
               "message": null
             },
             "archive_all": {
               "rule_name": "archive_all",
               "status": "Matched",
               "actions": [
                 {
                   "id": "archive",
                   "payload": {
                     "archive_type": "one",
                     "event": {
                       "created_ms": 123456,
                       "payload": {
                         "value_one": "something",
                         "value_two": "something_else"
                       },
                       "type": "the_event_type"
                     }
                   }
                 }
               ],
               "message": null
             }
           },
           "extracted_vars": {}
         }
       }
      }

Endpoint: match an event on a specific Tornado draft

-  HTTP Method: **POST**
-  path : **/api/v1_beta/event/drafts/{draft_id}/send**
-  request type: **JSON**
-  request/response example: same request and response of the
   **/api/v1_beta/event/current/send** endpoint

Tornado 'RuntimeConfig' Backend API
```````````````````````````````````

These endpoints allow inspecting and changing the Tornado
configuration at runtime. Please note that whatever configuration
change performed with these endpoints will be lost when Tornado is
restarted.

.. rubric:: Get the logger configuration

Endpoint: get the current logger level configuration

- HTTP Method: **GET**
- path: ``/api/v1_beta/runtime_config/logger``
- response type: **JSON**
- response example:

  .. code:: json

     {
       "level": "info",
       "stdout_enabled": true,
       "apm_enabled": false
     }

.. rubric:: Set the logger level


Endpoint: set the current logger level configuration

- HTTP Method: **POST**
- path: ``/api/v1_beta/runtime_config/logger/level``
- response: http status code 200 if the request was performed
  correctly
- request body type: **JSON**
- request body:

  .. code::   json

     {
       "level": "warn,
       tornado=trace"
     }

.. rubric:: Set the logger stdout output

Endpoint: Enable or disable the logger stdout output

- HTTP Method: **POST**
- path: ``/api/v1_beta/runtime_config/logger/stdout``
- response: http status code 200 if the request was performed
  correctly
- request body type: **JSON**
- request body:

  .. code:: json

     {
       "enabled": true
     }

.. rubric:: Set the logger output to Elastic APM

Endpoint: Enable or disable the logger output to Elastic APM

- HTTP Method: **POST**
- path: ``/api/v1_beta/runtime_config/logger/apm``
- response: http status code 200 if the request was performed
  correctly
- request body type: **JSON**
- request body:

  .. code:: json

     {
       "enabled": true
     }

.. rubric:: Set the logger configuration with priority to Elastic APM

Endpoint: This will disable the stdout and enable the Elastic APM
logger; in addition, the logger level will be set to the one provided,
or to "info,tornado=debug" if not present.

- HTTP Method: **POST**
- path: ``/api/v1_beta/runtime_config/logger/set_apm_priority_configuration``
- response: http status code 200 if the request was performed
  correctly
- request body type: **JSON**
- request body:

 .. code::  json

    {
      "logger_level": true
    }

.. rubric:: Set the logger configuration with priority to stdout

Endpoint: this will disable the Elastic APM logger and enable the
stdout; in addition, the logger level will be set to the one provided in
the configuration file.

- HTTP Method: **POST**
- path: ``/api/v1_beta/runtime_config/logger/set_stdout_priority_configuration``
- response: http status code 200 if the request was performed correctly
- request body type: **JSON**
- request body:

  .. code:: json

     {}

.. _tornado-runtime-smart-monitoring-status:

.. rubric:: Set the Smart Monitoring Executor status

Endpoint: this activates and deactivates the Smart Monitoring Executor.
When the Executor is in **active** state, it will execute incoming Actions as soon as
they are produced. When instead the Executor is in **inactive** state, all incoming
Actions will be queued until the Executor returns in **active** state.

- HTTP Method: **POST**
- path: ``/api/v1_beta/runtime_config/executor/smart_monitoring``
- response: http status code 200 if the request was performed correctly
- request body type: **JSON**
- request body:

  .. code:: json

     {
        "active": false
     }
