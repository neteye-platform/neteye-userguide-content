Shutdown Management Rest API
````````````````````````````

Shutdown can be executed on an host using a REST API. Currently, the
following calls are available.

Trigger Shutdown Definition
+++++++++++++++++++++++++++

Endpoint: **trigger-shutdown-definition**

This endpoint enables you to trigger an asynchronous run of a shutdown
definition via a REST API call. The call is non blocking and the
shutdown will be performed in background.

Parameters: \* **id**: The ID of the shutdown definition

Example:

.. code:: bash

   curl -u root:xxx -H 'Accept: application/json' https://localhost/neteye/shutdownmanager/api/trigger-shutdown-definition?id=1
