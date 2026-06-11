.. _adding-nodes:

Add a Filter Node
~~~~~~~~~~~~~~~~~

A Node should be added in order to create Tornado configuration:

#. Switch to Edit mode in the top right corner of your layout:

   When switching to Edit mode, a new draft is created on the fly if
   none is present, which is an exact copy of the running Tornado
   configuration. If not present in the draft, a root node of type
   **Filter** will be automatically added.

#. Click on the "Add" button in the top right corner and select the parent
   node to which you want to add a new node - a Filter or a Ruleset.

#. Optionally, click on the icon with the three dots on each node that
   from now on will be called the overflow menu

   .. figure:: ./../img/carbon/new-filter-node.png

     Adding a node

   All nodes at the same level are ordered alphabetically.

#. Define Filter node properties:

   -  ``filter name``: A unique string value should be only composed of letters,
      numbers and the "_" (underscore) character; it corresponds to the filename,
      stripped from its *.json* extension.
   -  ``description``
   -  ``active``: A boolean value; if ``false``, the Filter's children will
      be ignored.
   -  ``filter``: A boolean operator that, when applied to an event,
      returns ``true`` or ``false``. This operator determines whether an
      **Event** matches the **Filter**; consequently, it determines whether
      an **Event** will be processed by the Filter's inner nodes.

   .. figure:: ./../img/carbon/filter-properties.png

#. Define WHERE operator

   The operators in the WHERE tab allow you to configure a Filter node by specifying the condition
   for filtering only the events of a particular type, or, for example, events from a particular device within your network.
   All operator options are available in a dedicated tab of the Filter configuration form.

   .. figure:: ./../img/carbon/where.equals.png

   The node is using the same set of operators in 'WHERE' tab as a Ruleset node.

   You can find more details on each WHERE operator type in a dedicated :ref:`where-conditions` section.

If needed, you can delete a Node from the overflow menu when in *Edit mode*.


.. _tornado-filters-available-by-default:

.. rubric:: Filters available by default

The Tornado Processing Tree provides some out of the box Filters,
which match all, and only, the Events originated by some given tenant.
For more information on tenants in NetEye visit the :ref:`dedicated page <data-gathering-in-neteye-satellites>`.

These Filters are created at the top level of the Processing Tree,
in such a way that it is possible to set up tenant-specific Tornado pipelines.

Given for example a tenant named ``acme``, the matching condition of the
Filter for the ``acme`` tenant will be defined as:

.. code:: json

   {
       "type": "equals",
       "first": "${event.metadata.tenant_id}",
       "second": "acme"
   }

Keep in mind that these Filters must never be deleted nor modified, because they
will be automatically re-created.

.. note:: NetEye generates one Filter for :ref:`each tenant
   <neteye-satellite-configuration>`, including the default ``master``
   tenant.
