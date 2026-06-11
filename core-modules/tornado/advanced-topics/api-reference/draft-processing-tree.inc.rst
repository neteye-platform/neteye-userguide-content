.. _tornado-api-v2-draft-processing-tree:

Draft Processing Tree
=====================

.. _tornado-api-v2-create-draft:

Creating a Draft
----------------

This endpoint lets you create a new draft for the current active Processing Tree.

.. warning::

    If a draft already exists, this endpoint will overwrite the current draft. Make sure there's
    no currently active draft, before creating a new one.


**Url:** ``POST <base-url>/config/drafts/{param_auth}``

**Required Permission:** ``edit``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/my-auth" \
        -XPOST \
        -H "Authorization: Bearer <auth-token>"


**Result**

This endpoint returns the ``draft_id`` for the created draft.

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "id", "String", "The id for the newly created draft."


.. _tornado-api-v2-retrieve-drafts:

Retrieving All Available Drafts
-------------------------------

This endpoint lets you list out all available drafts for a specific authorization.

**Url:** ``GET <base-url>/config/drafts/{param_auth}``

**Required Permission:** ``view``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example:**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/my-auth" \
        -XGET \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns a list of draft_ids available for the selected authorization.


.. _tornado-api-v2-take-over-drafts:

Taking Over Draft
-----------------

This endpoint lets you take over the ownership of a draft from another user.

**Url:** ``POST <base-url>/config/drafts/{param_auth}/{draft_id}/takeover``

**Required Permission:** ``edit``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example:**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/my-auth/draft_001/takeover" \
        -XPOST \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns nothing if the takeover succeeded, otherwise it returns an error.


.. _tornado-api-v2-deploy-draft:

Deploying a Draft
-----------------

This endpoint lets you deploy a draft to be the new active Processing Tree. Note that this endpoint
only deploys the draft, and doesn't delete it automatically

**Url:** ``POST <base-url>/config/drafts/{param_auth}/{draft_id}/deploy``

**Required Permission:** ``edit``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example:**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/my-auth/draft_001/deploy" \
        -XPOST \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns nothing if the deploy succeeded, otherwise it returns an error.


.. _tornado-api-v2-delete-draft:

Deleting a Draft
----------------

This endpoint allows you to delete an existing draft.

**Url:** ``DELETE <base-url>/config/drafts/{param_auth}/{draft_id}``

**Required Permission:** ``edit``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example:**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/drafts/my-auth/draft_001" \
        -XDELETE \
        -H "Authorization: Bearer <auth-token>"

**Result:** This endpoint returns nothing if the deploy succeeded, otherwise it returns an error.


.. _tornado-api-v2-draft-get-node-details:

Get Draft Node Details
----------------------

Same as :ref:`tornado-api-v2-active-node-details`, but for a draft.

**Url:** ``GET <base-url>/config/draft/tree/details/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``view``


.. _tornado-api-v2-draft-get-node-children:

Get Draft Node Children
-----------------------

Same as :ref:`tornado-api-v2-active-child-nodes`, but for a draft.

**Url:** ``GET <base-url>/config/draft/tree/children/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``view``


.. _tornado-api-v2-draft-create-node:

Create Node in Draft
--------------------

Create a new child node of an existing node in the draft.

**Url:** ``POST <base-url>/config/draft/tree/details/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``edit``

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Type", "Source", "Description"
    ":ref:`tornado-api-v2-types-node-details`", "Body", "Pass the configuration of the node to create"


See also: :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl -XPOST \
        -H "Authorization: Bearer <auth-token>" \
        -H "Content-type: application/json" \
        --data '{"type": "Filter","name":"tenantA","active":true,"description":"","filter":{"type":"equals","first":"${event.metadata.tenant_id}","second":"tenantA"}}' \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/details/my-auth/draft_001/root"


**Result:** This endpoint returns nothing if the deploy succeeded, otherwise it returns an error.


.. _tornado-api-v2-import-child-node:

Import Child Node
-----------------

Import an exported configuration as a new child node.

**Url:** ``POST <base-url>/config/draft/tree/import/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``edit``

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Name", "Source", "Description"
    "file", "Body", "A file to upload"


**Example**

.. code-block:: bash

    curl -XPOST \
        -H "Authorization: Bearer <auth-token>" \
        -F 'file={"version":"1.0","Ruleset":{"name":"my_ruleset","rules":[]}}' \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/import/my-auth/draft_001/root"


**Result:** Same as :ref:`tornado-api-v2-draft-create-node`


Import Node to Replace
----------------------

This endpoint lets you replace an entire subtree with an exported version of the Tornado Processing Tree.

**Url:** ``PUT <base-url>/config/draft/tree/import/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``edit``

**Parameters:** Same as :ref:`tornado-api-v2-import-child-node`


Edit Node in Draft
------------------

To edit a node in the draft you can use this endpoint. The endpoint however only lets you edit
some details of the node. The operation will neither affect the children of the node, nor the
node's type.

.. warning::

    You always have to supply the full config of the node every time, even for editing just the
    node name. All the data will be overwritten every time.

**Url:** ``PUT <base-url>/config/draft/tree/details/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``edit``

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Type", "Source", "Description"
    ":ref:`tornado-api-v2-types-node-details`", "Body", "Pass the configuration of the node to create"

**Example**

.. code-block:: bash

    curl -XPOST \
        -H "Authorization: Bearer <auth-token>" \
        --data '{"name":"my_ruleset2"}' \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/details/my-auth/draft_001/root,master,my_ruleset"

**Result:** This endpoint returns nothing if the edit operation succeeds, otherwise it returns an error.


Get Rule
--------

Same as :ref:`tornado-api-v2-active-rule`, but for a draft.

**Url:** ``GET <base-url>/config/draft/rule/details/{param_auth}/{draft_id}/{ruleset_path}/{rule_name}``

**Required Permission:** ``view``

**Parameters:** See: :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/rule/details/my-auth/draft_001/root,master,my_ruleset/my_rule" \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns a :ref:`tornado-api-v2-types-rule` from the draft
if the node selected is a Ruleset node, a rule with the given name exists and the api-user has the
necessary permissions to access it.


Create Rule
-----------

Create a new rule in an existing ruleset.

**Url:** ``POST <base-url>/config/draft/rule/details/{param_auth}/{draft_id}/{ruleset_path}``

**Required Permission:** ``edit``

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Type", "Source", "Description"
    ":ref:`tornado-api-v2-types-rule`", "Body", "Path to the existing ruleset where to create the new rule"

See also: :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl -XPOST \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/rule/details/my-auth/draft_001/root,master,my_ruleset" \
        -H "Authorization: Bearer <auth-token>" \
        -H "Content-type: application/json" \
        -d '{"name":"my_rule","description":"","continue":true,"active":true,"constraint":{"WHERE":{"type":"AND","operators":[{"type":"contains","first":"${event.type}","second":"mail"}]},"WITH":{}},"actions":[{"id":"archive","payload":{"archive_type":"ARCHIVE TYPE","event":"${event}"}}]}'


**Result:** This endpoint returns nothing if the add operation succeeds, otherwise it returns an error.


Edit Rule
---------

Edit an existing rule.

**Url:** ``PUT <base-url>/config/draft/rule/details/{param_auth}/{draft_id}/{ruleset_path}/{rule_name}``

**Required Permission:** ``edit``

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Type", "Source", "Description"
    ":ref:`tornado-api-v2-types-rule`", "Body", "Path to the existing ruleset where to find the rule to edit"

See also: :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl -XPUT \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/rule/details/my-auth/draft_001/root,master,my_ruleset/my_rule" \
        -H "Authorization: Bearer <auth-token>" \
        -H "Content-type: application/json" \
        -d '{"name":"my_rule","description":"","continue":true,"active":true,"constraint":{"WHERE":{"type":"AND","operators":[{"type":"contains","first":"${event.type}","second":"mail"}]},"WITH":{}},"actions":[{"id":"archive","payload":{"archive_type":"ARCHIVE TYPE","event":"${event}"}}]}'


**Result:** This endpoint returns nothing if the edit operation succeeds, otherwise it returns an error.


Delete Rule
-----------

Delete an existing rule.

**Url:** ``DELETE <base-url>/config/draft/rule/details/{param_auth}/{draft_id}/{ruleset_path}/{rule_name}``

**Required Permission:** ``edit``

**Parameters** See: :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl -XDELETE \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/rule/details/my-auth/draft_001/root,master,my_ruleset/my_rule" \
        -H "Authorization: Bearer <auth-token>"

**Result:** This endpoint returns nothing if the delete operation succeeds, otherwise it returns an error.


Reorder Rule in Ruleset
-----------------------

Reorder an existing rule in relation of the other rules of the same ruleset.

**Url:** ``PUT <base-url>/config/draft/rule/move/{param_auth}/{draft_id}/{ruleset_path}/{rule_name}``

**Required Permission:** ``edit``

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Type", "Source", "Description"
    "ruleset_path", "URL", "Path to the existing ruleset where to create the new rule"
    ":ref:`tornado-api-v2-types-rule-position`", "Body", "New position of the rule in the ruleset"

**Example**

.. code-block:: bash

    curl -XPUT \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/rule/move/my-auth/draft_001/root,master,my_ruleset/my_rule" \
        -H "Authorization: Bearer <auth-token>" \
        -H "Content-type: application/json" \
        -d '{"position":"3"}'


**Result:** This endpoint returns nothing if the edit operation succeeds, otherwise it returns an error.


Export Subtree
--------------

With this Endpoint you can export a subtree of tornado, starting from a given base node.

**Url:** ``PUT <base-url>/config/draft/tree/export/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``view``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl -XGET \
        -H "Authorization: Bearer <auth-token>" \
        -o export.json \
        "http://tornado.neteyelocal:4748/api/v2_beta/config/draft/tree/export/my-auth/draft_001/root"



**Result:** Returns a `HTTP file attachment <https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition>`_
containing the config of the entire subtree.


Delete Draft Node
-----------------

Delete a Node in the Draft, along with all of its children.

**Url:** ``DELETE <base-url>/config/draft/tree/details/{param_auth}/{draft_id}/{node_path}``

**Required Permission:** ``edit``

**Result:** This endpoint returns nothing if the request succeeded, otherwise it returns an error.
