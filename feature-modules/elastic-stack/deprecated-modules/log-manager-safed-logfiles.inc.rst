.. _logmanager-safed-logfiles:

Safed Configuration: LogFile Templates, Objectives and Filters
--------------------------------------------------------------

A LogFile Template allows you to associate one or more LogFile
Objectives with one or more LogFile Filters. To define a new LogFile
Template, click on **Log Manager > Safed Agent > LogFile Templates**.
Then click the "Add" action, and specify:

- **Name:** A name for the Objective/Filter combination.
- **Description:** A more extended description of the template.

The two sets of subpanels below allow you to associate LogFile
Objectives and LogFile Filters with the current LogFile Template. The
left side of each subpanel contains defined objectives and filters that
are available. On the right are those that have been associated with the
current template. To move elements from one side to the other, you can
use the  multi-select tool.

.. _figure-logfile-template-add:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logfile-template-add-01.png
   :alt: New LogFile Template definition

   Adding a new LogFile Template definition.

To edit an existing LogFile template, follow the same sequence as
above.  You will see a list of all existing LogFile templates as shown
in :numref:`figure-logfile-template-list`. By clicking on the name of
a particular template, you will be able to edit it using the same
panel above used to create it.

.. todo:: fix icon

The listing also shows the number of General Settings items that have
been associated with this template. If there are no associations, you
can delete a filter by clicking on the trash can ( ) icon to the right.
If instead the template is associated with an objective or filter, the
trash can icon will change to black to indicate the template cannot be
deleted.

.. _figure-logfile-template-list:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logfile-template-edit-01.png
   :alt: List of existing LogFile Template definitions

   List of existing LogFile Template definitions

Creating, Editing and Deleting a LogFile Objective Definition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To define a new LogFile Objective, click on :menuselection:`Log
Manager --> Safed Agent --> LogFile Templates --> LogFile
Objectives`. Then click the "Add" action, specifying:

-  **Name:** A descriptive name for the LogFile Objective configuration.
-  **Log File or Directory:** Either the path to the log file, or the
   directory where dynamically named logs are kept.
-  **Dynamic Log name format:** If the field above is a directory, this
   field should capture the filename's structure. A percent sign ('%')
   represents a date in the form YYMMDD, and regular expressions can be
   used. If this field is empty, then the first file in the directory
   will be selected.
-  **Include comment lines:** Determines whether comment lines (those
   beginning with a '#') will be included.


.. _figure-logfile-objective-new:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logfile-objective-add-01.png
   :alt: Adding a new LogFile Objective definition

   Adding a new LogFile Objective definition.

To edit an existing LogFile objective, follow the same sequence as
above. You will see all existing LogFile objectives along with a quick
summary as shown in :numref:`figure-logfile-objective-edit`. Instead
of clicking on the "Add" action, click on the name of an existing
objective. You can then edit the objective using the same panel above
used to create it (:numref:`figure-logfile-objective-new`).

.. todo:: fix icon

The summary also shows the number of LogFile templates that have been
associated with this objective. If there are no such associations, you
can delete an objective by clicking on the trash can ( ) icon.

.. _figure-logfile-objective-edit:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logfile-objective-edit-01.png
   :alt: Index of existing LogFile Objective definitions

   Index of existing LogFile Objective definitions.

Creating, Editing and Deleting a LogFile Filter Definition
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To define a new LogFile Filter, click on **Log Manager > Safed Agent >
LogFile Templates > LogFile Filters**. Then click the "Add" action,
specifying:

- **Filter name:** A descriptive name for the filter.
- **Include/Exclude:** Whether this filter definition should act in
  inclusive or exclusive mode.
- **Filter Regex:** A regex-compatible filter expression, containing
  for instance:

  -  A Boolean OR: 'root\|administrator'
  -  Grouping parentheses: 'gr(a\|e)y'
  -  Quantification: ? = zero or one; \* = zero or more

.. _figure-logfile-filter-add:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logfile-filter-add-01.png
   :alt: Adding a new LogFile Filter definition

   Adding a new LogFile Filter definition.

To edit an existing LogFile filter, follow the same sequence as above.
You will see all existing LogFile filters along with a quick summary
as shown in :numref:`figure-logfile-filter-edit`. By clicking on the
name of a particular filter, you will be able to edit the filter using
the same panel above used to create it.

.. todo:: fix icon

The summary also shows the number of LogFile templates that have been
associated with this filter. If there are no such associations, you can
delete a filter by clicking on the trash can ( ) icon.

.. _figure-logfile-filter-edit:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logfile-filter-edit-01.png
   :alt: Index of existing LogFile Filter definitions

   Index of existing LogFile Filter definitions.
