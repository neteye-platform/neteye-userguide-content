.. _tornado-api-v2-overview:

Overview
========

The Tornado API lets you automate all the tasks that are available for execution over the WEB-GUI by
calling the endpoints directly. In this section we will go over all the available endpoints and how
to use them to query and modify the Tornado Processing Tree.

All available endpoints can be found by default under ``http://tornado.neteyelocal:4748/api/v2_beta/``


Authorization
-------------

To perform any action in Tornado you need to provide an authorization token with the correct
permissions.The token is a base64 encoded JSON object, which contains the username as well as a map
of available authorizations.

.. _tornado-api-v2-auth-permissions:

Permissions
~~~~~~~~~~~

To interact with any parts of the tree you need to be granted a corresponding permission. The
following permissions are available in |ne|:

.. csv-table::
    :widths: 30, 70
    :header-rows: 1

    "Permission", "Description"
    "view", "Allows you to retrieve any information about the active Processing Tree"
    "edit", "Allows you to modify drafts"
    "test_event_execute_actions", "Allows you to  execute action when sending test events"
    "admin", "Gives you full access to all features of the Tornado api"


.. _tornado-api-v2-auth-token:

Authorization Token
~~~~~~~~~~~~~~~~~~~

An Authorization Token can contain multiple authorizations. The Authorization is composed of a set
of permission as well as a path for which those permissions apply. The path needs to start with
``root`` and gives the path to any node in the Processing Tree. Each following node must then be a
new element in the list. The api-user is then prohibited to modify any node outside of that base-path.

Please be aware that the last node of the auth path will be the new root node for all api calls.
For instance, if the auth path restricts the user to ```root, master``, the URL path to the node
must start from the ``master``, omitting the ``root`` node.

The ``roles`` property of the authotization is a set of :ref:`tornado-api-v2-auth-permissions` to
which the api-user is restricted.

.. code:: json

    {
        "user": "<my-tornado-user>",
        "auths": {
            "my-auth": {
                "path": [ "root", "master", "email" ],
                "roles": [ "view", "edit" ]
            }
        }
    }

.. note::

    The name in the authorization MUST not be empty. An empty name will result in the 401
    HTTP error.


To see how to generate the token, take a look at the :ref:`tornado-api-v2-getting-started` section.


**Best practices**

Give every api-user a distinct and recognizible name. This lets you track changes and detect
clashes during the editing of the draft.

Give every api-user the minimal permission necessary to perform its task. For example, if a api
client only needs to read / export config from one specific tenant, give the client only read
permissions for their specific tenant node.


.. _tornado-api-v2-common-request-types:

Common Request Types
~~~~~~~~~~~~~~~~~~~~

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Name", "Source", "Description"
    "param_auth", "Path", "The authorization from the header to use."
    "node_path", "Path", "A comma separated list of node name that points to a specific node, starting from the root node of the tenant."
    "rule_name", "Path", "The name of the Rule you want to access"
    "draft_id", "Path", "The id of the draft you want to take over for your user"


Limitations
-----------

Tornado does not yet support Multitenancy for the draft editing. Your Tornado installation has
exactly one active Processing Tree and zero to one drafts with a dedicated owner of said draft.
If another user already owns the draft or another api-client or user takes over the draft, during
execution, then future requests will be rejected by the endpoint.

There is no reliable way right now to detect whether a draft contains any changes to the current
active Processing Tree. So you might need to take over the existing draft, overwrite it, or ask the
user to deploy and delete the current draft before you can continue.
