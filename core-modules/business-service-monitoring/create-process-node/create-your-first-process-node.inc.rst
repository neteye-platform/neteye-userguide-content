.. _bp-create-first-node:

Create your first Business Process Node
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A *Business Process Node* consists of a *name*, *title*, an *operator*
and one or more child nodes. Each node can be a Root Node, a child node
of other Business Process Nodes, or both. When you first create a
Business Process, there are no nodes available, so you will be prompted to
create one.


Configuring Your First Node
```````````````````````````

To create our first *Business Process Node*, click on the *Add* button.
You will then see the node configuration form (an asterisk indicates a
required field):

.. _figure-bp-new-node-added:

.. figure:: /core-modules/business-service-monitoring/img/add-new-node.png

   The Business Process node configuration form.

Configuring a Business Process requires filling in the following fields:

-  *ID:* The first setting is the identifier that must
   be unique across all nodes that will be defined. This identifier will
   be used within every URL and also within any *Check Commands*
   referring to this node by an Icinga *Service Check*.

-  *Display Name:* (Optional) As uniqueness sometimes leads to
   not-so-beautiful names, you can also specify a display title which
   will be used across all dashboards.

-  *Operator:* A Business Process’ operator defines its behavior,
   specifying how its own state will be calculated. It can be one of
   AND, OR, NOT, DEG or MIN.

-  *Visualization:* This describes when the node should be displayed.
   Since this node will be a *root node*, the form automatically
   suggests that you create a *Toplevel Process*. Alternatively, you
   could also create a *Subprocess*. As we are currently not adding this
   node to another node, that choice would lead to an *Unbound Node*
   that could be linked to later on.

-  *Info URL:* (Optional) You might also want to provide a link to
   additional information related to a specific process. This could be
   instructions with more technical details, or hints explaining what
   should happen if an outage occurs. You might not want to do so for
   every single node, but it might come in handy for your most important
   (e.g., top level or high priority) nodes.

Completed Business Process Node
```````````````````````````````

That's it, you are ready to submit the form. After clicking the green
“Submit” button, you should see your first completed Business Process
Node. A red notification as in  :numref:`figure-bp-first-node-created`
reminds you that your pending
changes have not been stored yet.

.. _figure-bp-first-node-created:

.. figure:: /core-modules/business-service-monitoring/img/first-node-created.png

   The completed business process node, “My Root BP Node”.

You can now *Store the Configuration* as is, or move on to add
additional nodes to continue your configuration. In the latter case, you
will also need to select a *Node type*. This can be either:

-  *Existing Process (Node):* When you want to link this node to another
   node you have already created. Choose the appropriate existing node
   from the drop down list, then click the green “Submit” button. (Be
   sure not to link this node to itself.)

-  *New Process Node:* Here you can create a new, independent root-level
   node.

-  *Host:* When you have created the lowest level of intermediate nodes
   in your business process hierarchy, you can add the hosts that you
   would like to monitor as part of that business process.

-  *Service:* For each host in your business process hierarchy, you can
   specify particular services on the host that are part of the business
   process, and thus exclude other, irrelevant services on that host.

- *Hosts from filter:* You can select one or more hosts in a single
   operation to be added to your business process, starting from a
   monitoring filter expression (e.g., `host_name=*linux*`).

- *Services from filter:* You can select one or more services in a single
   operation to be added to your business process, starting from a
   monitoring filter expression (e.g., `service_description=*disk*`).

.. note:: While you can create Business Process nodes whenever you
   like, you cannot add Host and Service nodes until you have created
   the underlying hosts and services themselves. Thus you should
   complete that step before setting up your business process
   hierarchy.

To edit a node you have already created, click on the “Unlock” button
and then choose the “Notepad” icon on the node you wish to change.

.. note:: The grey arrows below the Business Process title are
   breadcrumbs that show your current position. Their use is described
   in more detail in the `official Business Process documentation
   <https://icinga.com/docs/icinga-business-process-modelling/latest/doc/12-Web-Components-Breadcrumb/>`_.
