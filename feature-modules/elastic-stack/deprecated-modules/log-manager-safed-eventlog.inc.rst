.. _logmanager-safed-eventlog:

Safed Configuration: EventLog Templates and Objectives
------------------------------------------------------

A new EventLog is created by setting up a new EventLog Template and then
associating it with one or more EventLog Objectives. To define a new
EventLog Template, click on **Log Manager > Safed Agent > EventLog
Templates**. Then click the "Add" action, and specify:

-  **Name:** A name for the Objective/Filter combination.
-  **Description:** A more extended description of the template.

The two sets of subpanels below allow you to add the desired EventLog
Objectives. The left side of the subpanel contains the available
objectives you have defined. On the right are those that have been
associated with the current template. To move elements from one side
to the other, you can use the multi-select tool,
including changing the relative priority
among the various EventLog objectives.

.. _figure-eventlog-template-add:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-eventlog-template-add-01.png
   :alt: Adding a new EventLog Template definition

   Adding a new EventLog Template definition.

To edit an existing EventLog template, follow the same sequence as
above. You will see a list of all existing EventLog templates as shown
in :numref:`figure-eventlog-template-edit`. By clicking on the name of
a particular template, you will be able to edit it using the same
panel above used to create it.

.. todo:: fix icon

The listing also shows the number of General Settings items that have
been associated with this template. If there are no associations, you
can delete a filter by clicking on the trash can ( ) icon to the right.

.. _figure-eventlog-template-edit:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-eventlog-template-edit-01.png
   :alt: List of existing EventLog Template definitions

   List of existing EventLog Template definitions.

Creating, Editing and Deleting an EventLog Objective Definition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To define a new EventLog Objective, click :menuselection:`Log Manager
--> Safed Agent --> EventLog Templates --> EventLog Objectives`. Then
click the "Add" action, specifying:

- **Name:** A descriptive name for the EventLog objective
  configuration.
- **High Level Event Type:** This selects pre-defined categories of
  events that can depend on the installed operating system, such as
  authentication, file access, user rights changes, etc.
- **General Filter Expression:** A regex-compatible filter expression
  applied to EventLog items. The *Include/Exclude* option will affect
  this globally. Otherwise, an expression can contain for instance:

  - A Boolean OR: 'root\|administrator'
  - Grouping parentheses: 'gr(a\|e)y'
  - Quantification: ? = zero or one; \* = zero or more

- **Event ID:** If the *High Level Event Type* is not set, you can
  insert a specific set of Event IDs to use instead of the pre-defined
  events. If it is set, then the high level events will take priority.
  You can also specify whether to **include** just those events, or
  **exclude** them from the set of all events.
- **User Filter Expression:** A regex-compatible filter applied to the
  user(s) associated with an Event. Again, this can be globally
  included or excluded.
- **Event Types to include:** In addition to high level types, this
  multi-select tool allows you to
  include only specific event types such as those based on security or
  severity.
- **Event Log to include:** This option allows you to select the types
  of logs to include rather than the type of events. If you select
  *Custom*, a text field will appear allowing you to enter a specific
  log.
- **Alert level:** Here you can specify a maximum alert level to
  include. The Log Manager will then include all alerts of that
  severity or lower.
- **Additional comment:** In this text box you can write comments such
  as explaining what this EventLog objective is intended for, or which
  Event IDs are covered.
- **Register Templates:** Any EventLog templates you have defined can
  be included here.

.. _figure-eventlog-objective-add:

.. image:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-eventlog-objective-add-03.png
.. image:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-eventlog-objective-add-03.png
.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-eventlog-objective-add-03.png

   Adding a new EventLog objective definition.

To edit an existing EventLog objective, follow the same sequence as
above. You will see all existing EventLog objectives along with a quick
summary as shown in Figure 4. Instead of clicking on the "Add" action,
click on the name of an existing objective. You can then edit the
objective using the same panel above (Figure 3) used to create it.

The summary also shows the number of EventLog templates that have been
associated with this objective. If there are no such associations, you
can delete an objective by clicking on the trash can ( ) icon. In
addition, there is a clone ( ) button that can save you time whenever
you need to define multiple similar EventLog objectives.

.. _figure-eventlog-objective-edit:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-eventlog-objective-edit-01.png
   :alt: Index of existing LogFile Objective definitions

   Index of existing LogFile Objective definitions.
