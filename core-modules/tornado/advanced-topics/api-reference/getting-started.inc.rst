.. _tornado-api-v2-getting-started:

Getting Started
===============

This section will help to get started with using the Tornado API to edit the Processing Tree
through the whole life cycle of a draft. First we'll create a draft, add a node, edit that node
and then deploy the draft so that our changes will end up in the actual environment.

First of all, we need an authorization token, so that Tornado lets us use the API. That token
contains a unique name and its permissions. The name for our client will be ``api-filter-manager``,
since it will create a new filter for the master tenant. Since we are only operating on the master
node, we restrict the token to that path, so we can't accidentally edit something else.

.. code-block:: bash

    TOKEN_CONTENT='{"user":"api-filter-manager","auths":{"master-auth":{"path":["root","master"],"roles":["view","edit"]}}}'
    AUTH_TOKEN=$(echo "$TOKEN_CONTENT" | base64 --wrap=0)


Before we can edit the draft, we need to make sure that the draft is not being edited at the moment
by another user. To do that we make a request to check that no drafts are currently present:

.. code-block:: bash

    curl -XGET \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/master-auth"


This request should return an empty array. If not, it's safest to ask the user to deploy and/or
delete the current draft before continuing, so that the execution does not interfere with any
ongoing modifications to the draft.

Now that we are sure we have a clean slate to work on, we first need to create our own draft. For
this, we make a POST request to the appropriate endpoint:

.. code-block:: bash

    RESULT=$(curl -XPOST \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/master-auth")
    DRAFT_ID=$(echo $RESULT | jq .id --raw-output)


We now have successfully created the draft we want to work on. Next we are going to actually create
a new node in the Processing Tree and then edit it. First, we make a POST request to create a new
filter node as a child of the master tenant. We just provide the new name for now.

.. code-block:: bash

    curl -XPOST \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        -H "Content-type: application/json" \
        --data '{"type": "Filter","name":"meaning_of_life","active":true,"description":"","filter":null}' \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/details/master-auth/$DRAFT_ID/master"


Next, let's add a filter to the filter node, so that the children only receive the event designated
to them. We'll do that by first fetching the data for that node from the API, editing it and then
send the edited node to Tornado. The creation could be done in one step, but this helps to better
illustrate the interaction between the APIs.

.. code-block:: bash

    FILTER_NODE=$(curl -XGET \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/details/master-auth/$DRAFT_ID/master,meaning_of_life")

    FILTER_NODE=$(echo "$FILTER_NODE" | jq '.filter = {"type":"AND","operators":[{"type":"equals","first":"${event.payload.meaning_of_life}","second":42}]}')

    curl -XPUT \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        -H "Content-type: application/json" \
        --data "$FILTER_NODE" \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/details/master-auth/$DRAFT_ID/master,meaning_of_life"


Now that we have the node ready for use, we can import some base config we want the filter node to
have. We assume this configuration is located in the file :file:`filter-base-config.json` located
in the current working directory.

.. code-block:: bash

    curl -XPOST \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        -F 'file=@filter-base-config.json' \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/import/master-auth/$DRAFT_ID/master,meaning_of_life"


Now we finally configured our new filter how we wanted. The last step is just to deploy the whole
configuration so that it can pick up work in the active production environment. After the deploy, we
will remove the old draft so that the next user has a clean slate, when he wants to further edit
the Tornado config.

.. code-block::

    curl -XPOST \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/master-auth/$DRAFT_ID/deploy"

    curl -XDELETE \
        -H "Authorization: Bearer $AUTH_TOKEN" \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/master-auth/$DRAFT_ID"


With that we have completed the life cycle of a Tornado config draft. The new filter is now ready
to process events for Tornado. For further use, check out all the following Documented API
endpoints.
