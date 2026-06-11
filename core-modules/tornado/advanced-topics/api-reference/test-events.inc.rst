.. _tornado-api-v2-test-events:

Test Event API
==============

.. _tornado-api-v2-test-event-active-tree:

Send Test Event To Active Processing Tree
-----------------------------------------

This endpoint lets you process a test event with the current active Tornado Processing Tree.

**Url:** ``POST /api/v2_beta/event/active/{param_auth}``

**Required Permissions:** ``view`` OR ``edit``

**Optional Permissions:** ``test_event_execute_actions`` to execute the actions if they match

**Parameters**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Name", "Source", "Description"
    "param_auth", "Path", "The authorization from the header to use."
    "event", "Body", "A :ref:`tornado event <tornado-collector-event-properties>` to process with the Processing Tree"
    "process_type", "Body", "Either ``SkipActions`` or ``Full``. Whether the actions should be executed. If this is set, then the api user MUST have the ``test_event_execute_actions`` permission"

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/event/active/my-auth" \
    -H "Authorization: Bearer <auth-token>" \
    -XPOST \
    --data '{"event":{"type":"the_event_type","created_ms":123456,"payload":{}},"process_type":"Full"}'

**Result**

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "event", ":ref:`tornado-collector-event-properties`", "The initial event that was sent over the request"
    "result", ":ref:`tornado-api-v2-event-processed-node`", "The Processing Tree, starting from the auth root, enriched with data about how the event was processed."


Send Test Event To Draft Processing Tree
----------------------------------------

Process a test event with a draft Tornado Processing Tree.

**Url:** ``POST /api/v2_beta/event/draft/{param_auth}/{draft_id}``

**Required Permissions:** ``view`` OR ``edit``

**Optional Permissions:** ``test_event_execute_actions`` to execute the actions if they match

**Parameters:** Same as :ref:`tornado-api-v2-test-event-active-tree`

**Example**

.. code-block:: bash

    curl "http://tornado.neteyelocal:4748/api/v2_beta/event/draft/my-auth/draft_001" \
    -H "Authorization: Bearer <auth-token>" \
    -XPOST \
    --data '{"event":{"type":"the_event_type","created_ms":123456,"payload":{}},"process_type":"Full"}'

**Result:** Same as :ref:`tornado-api-v2-test-event-active-tree`


.. _tornado-api-v2-event-types:

Test Event Types
----------------


.. _tornado-api-v2-event-processed-node:

ProcessedNode
~~~~~~~~~~~~~

The content of the processed node vary by node type.

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "type", "String", "Either Filter or Ruleset"
    "name", "String","The name of the node"


ProcessedFilterNode
~~~~~~~~~~~~~~~~~~~

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "filter.status", "String", "On of Matched, NotMatched or Ignored"
    "nodes", ":ref:`tornado-api-v2-event-processed-node`\[\]", "A list of the filters child nodes. (Only populated if filter status is Matched)"


ProcessedRulesetNode
~~~~~~~~~~~~~~~~~~~~

.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "rules.rules", ":ref:`tornado-api-v2-event-processed-rule`\[\]", "A list of all the rules in the ruleset with extra processing information."
    "rules.extracted_vars", "Object", "A map of all extracted variables "


.. _tornado-api-v2-event-processed-rule:

ProcessedRule
~~~~~~~~~~~~~


.. csv-table::
    :widths: 15, 15, 70
    :header-rows: 1

    "Property", "Type", "Description"
    "name", "String", "The name of the Rule"
    "status", "String", "On of Matched, NotMatched, PartiallyMatched or NotProcessed"
    "action", ":ref:`tornado-api-v2-types-action`\[\]", "A list of all actions performed by the rule"
    "message (Optional)", "String", "An error message if a rule was only partially matched"
    "meta.actions (Optional)", "Object[]", "A list of resolved rules, with extra metadata for the processing"

.. ToDo: Document ActionMetaData Type more in detail?
