.. _tornado-api-v2-active-processing-tree:

Active Processing Tree
======================

The Tornado API provides you an easy way to traverse your Processing Tree.


.. _tornado-api-v2-active-tree-info:

Get Tree Info
-------------

This endpoint lets you fetch a small summary about the subtree of the tenant.

**Url:** ``GET <base-url>/config/active/tree/info/{param_auth}``

**Required Permission:** ``view``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example:**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/active/tree/info/my-auth" \
        -H "Authorization: Bearer <auth-token>"


**Result**

This endpoint returns a JSON object with the following properties:

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "filters_count", "Integer", "The total number of filters in the whole subtree"
    "iterators_count", "Integer", "The total number of iterators in the whole subtree"
    "rules_count", "Integer", "The total number of rules in the whole subtree"


.. _tornado-api-v2-active-node-details:

Get Node Details
----------------

This endpoints lets you fetch all information about a single node in the currently active
Processing Tree.

**Url:** ``GET <base-url>/config/active/tree/details/{param_auth}/{node_path}``

**Required Permission:** ``view``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/active/tree/details/my-auth/root,master" \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns a JSON object of type :ref:`tornado-api-v2-types-node-details` if
a node exists in the given path and the api-user has permission to access it.


.. _tornado-api-v2-active-child-nodes:

Get Child Nodes
---------------

This endpoints lets you fetch basic information about all the child nodes of a specific filter node.

**Url:** ``GET <base-url>/config/active/tree/details/{param_auth}/{node_path}``

**Required Permission:** ``view``

**Parameters:** See :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/active/tree/details/my-auth/root,master" \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns a JSON list of :ref:`tornado-api-v2-types-node-info` if the node
selected is a filter node and the api-user has permission to access it.


.. _tornado-api-v2-active-rule:

Get Rule
--------

This endpoints lets you fetch all information about a single Rule in the currently active
Processing Tree.

**Url:** ``GET <base-url>/config/active/rule/details/{param_auth}/{ruleset_path}/{rule_name}``

**Required Permission:** ``view``

**Parameters:** See: :ref:`tornado-api-v2-common-request-types`

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/config/active/rule/details/my-auth/root,master,my_ruleset" \
        -H "Authorization: Bearer <auth-token>"


**Result:** This endpoint returns a :ref:`tornado-api-v2-types-rule` from the Processing Tree
if the node selected is a Ruleset node, a rule with the given name exists and the api-user has the
necessary permissions to access it.
