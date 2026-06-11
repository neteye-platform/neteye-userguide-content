.. _tornado-api-v2-types:

Tornado API Common Types
========================

In this section you can find all types that are either accepted or returned by the Tornado Api.

.. _tornado-api-v2-types-rule:

Rule
----

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "name", "String", "The name of the rule"
    "description", "String", "A short description of the rule provided by the user"
    "continue", "Boolean", "Determines whether the rule processing should be terminated if the rule matches"
    "active", "Boolean", "Determines whether the rule is active at the moment"
    "constraint", "[]", "A list of all the constraint that determine the actions to be performed or not"
    "actions", "[]", "A list of all the actions of the rule"


**Example:**

.. code-block:: json

    {
        "name": "vmdevents",
        "description": "Log and propagate vmd-alarms.",
        "continue": true,
        "active": true,
        "constraint": {
            "WHERE": {
                "type": "AND",
                "operators": [
                    {
                        "type": "equals",
                        "first": "${event.payload.foo}",
                        "second": "bar"
                    }
                ]
            },
            "WITH": {
                "variable_1": {
                    "from": "${event.payload}",
                    "regex": {
                        "type": "Regex",
                        "match": ".*",
                        "group_match_idx": null,
                        "all_matches": false
                    },
                    "modifiers_post": [
                        {
                            "type": "Lowercase"
                        },
                        {
                            "type": "ReplaceAll",
                            "find": "foo",
                            "replace": "bar",
                            "is_regex": false
                        }
                    ]
                }
            }
        },
        "actions": [
            {
                "id": "elasticsearch",
                "payload": {
                    "auth": {
                        "jwc": true
                    },
                    "data": {
                        "id": 2
                    },
                    "endpoint": "elasticsearch.neteyelocal",
                    "index": "foobar"
                }
            }
        ]
    }

.. _tornado-api-v2-types-rule-details:

RuleDetails
-----------

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "name", "String", "The name of the rule"
    "description", "String", "A short user provided description of the rule"
    "continue", "Boolean", "Determines whether the rule processing should be terminated if the rule matches"
    "active", "Boolean", "Determines whether the rule is active at the moment"
    "actions", "String[]", "A list of the names of all actions the rule will perform"


**Example:**

.. code-block:: json

    {
        "name": "vmdevents",
        "description": "Log and propagate vmd-alarms.",
        "continue": true,
        "active": true,
        "actions":[ "archive", "script" ]
    }

.. _tornado-api-v2-types-rule-position:

RulePosition
------------

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "position", "Integer", "The new position of the rule in the ruleset"


**Example:**

.. code-block:: json

    {
        "position": "3"
    }

.. _tornado-api-v2-types-operator:

Operator
--------

The operators are the same as in the config. You can learn all about them in the Tornado
:ref:`where-conditions` section of the userguide


.. _tornado-api-v2-types-node-info:

NodeInfo
--------

The node info gives a quick summary of a node in the Processing Tree. To distinguish between the
node types, the :ref:`tornado-api-v2-types-node-details` Type always contains the property ``type``
that can be one of ``Ruleset``, ``Iterator`` or ``Filter``. Depending on this value the type can
have the following properties:

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "name", "String", "The name of the filter"
    "rules_count", "Integer", "The total number of rules in all the subtree"
    "description (Filter/Iterator)", "String", "A short description of the node provided by the user"
    "children_count (Filter/Iterator)", "Integer", "The number of direct children of the node"
    "has_iterator_ancestor (Filter)", "Boolean", "Whether the filter is a child or descendant of an iterator node at any point in the processing tree"
    "active (Filter/Iterator)", "Boolean", "Determines whether the node is active at the moment"


**Example:**

.. code-block:: json

    {
        "type":"Filter",
        "name":"api_neteye_cloud",
        "rules_count":1,
        "children_count":1,
        "description":"Used for signaling failures in the api.neteye.cloud endpoint",
        "active":true
    }

.. _tornado-api-v2-types-node-details:

NodeDetails
-----------

The node details can either be details for a Ruleset, an Iterator or a Filter. To distinguish
between the two, the :ref:`tornado-api-v2-types-node-details` Type always contains the property
``type`` that can be ``Ruleset``, ``Iterator`` or ``Filter`` respectively. Depending on this value
the type can have the following properties:


.. _tornado-api-v2-types-filter-node-details:

FilterNodeDetails
~~~~~~~~~~~~~~~~~

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "type", "String", "Always set to ``Filter``"
    "name", "String", "The name of the filter"
    "description", "String", "AA short description of the filter provided by the user"
    "active", "Boolean", "Determines whether the filter is active at the moment"
    "filter (Optional)", ":ref:`tornado-api-v2-types-operator`", "The operators the rule matches against"


**Example:**

.. code-block:: json

    {
        "type": "Filter",
        "name": "master",
        "description": "Events from tenant: master. [..]",
        "active": true,
        "filter": {
            "type":"equals",
            "first":"${event.metadata.tenant_id}",
            "second":"master"
        }
    }


.. _tornado-api-v2-types-iterator-node-details:

IteratorNodeDetails
~~~~~~~~~~~~~~~~~~~

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "type", "String", "Always set to ``Iterator``"
    "name", "String", "The name of the iterator"
    "description", "String", "A short description of the iterator provided by the user"
    "active", "Boolean", "Determines whether the iterator is active at the moment"
    "target", "String", "An Accessor Expression defines the field of an event to iterate over"


**Example:**

.. code-block:: json

    {
        "type": "Iterator",
        "name": "Iterator_Node",
        "description": "Iterates over all alerts",
        "active": true,
        "target": "${event.payload.data.alerts}"
    }


.. _tornado-api-v2-types-ruleset-node-details:

RulesetNodeDetails
~~~~~~~~~~~~~~~~~~

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "type", "String", "Always set to ``Ruleset``"
    "name", "String", "The name of the filter"
    "rules", ":ref:`tornado-api-v2-types-rule-details`", "A list containing basic information on the rules in the Ruleset."

**Example:**

.. code-block:: json

    {
        "type": "Ruleset",
        "name": "api_neteye_cloud",
        "rules": [
            {
                "name": "foobar",
                "description": "",
                "continue": true,
                "active": true,
                "actions": [
                    "elasticsearch"
                ]
            },
            {
                "name": "foo",
                "description": "",
                "continue": true,
                "active": true,
                "actions": [
                    "script"
                    "archive"
                ]
            }
        ]
    }

.. _tornado-api-v2-types-action:

Action
------

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "id", "String", "The action name to execute"
    "payload", "Object", "The contents of the action"


**Example:**

.. code-block:: json

    {
        "id":"archive",
        "payload":{
            "archive_type":"vmd",
            "event":"${event.payload}"
        }
    }
