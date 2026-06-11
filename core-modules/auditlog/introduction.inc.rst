.. _auditlog-introduction:

Audit Log Introduction
----------------------

The Audit Log allows administrators to look back at changes that have
been made to NetEye. This is useful should you need to troubleshoot a
problem, or if you need to keep a record of important events, such as
changes to global permissions.

The Audit Log module centrally records information about events such as
the creation, modification, deletion or deployment of module objects for
all those modules that implement an object structure that is compatible
with this module.

To view the Audit Log, select :menuselection:`System / Audit Log`.

Features
~~~~~~~~

The Audit Log makes (compatible) configuration changes persistent in a
database, including:

* An ID of the modified object
* The user that made the change
* The type of action (*Create*, *Modify*, *Delete* and Deploy*)
* The type of object that was changed
* A copy of the new object (for *Create* and *Modify*)
* The module that originated the change
* The time and date the change was made

Audit Log can also show the differences between the previous and new
versions of an object in a separate panel.

NetEye’s permissions system allows administrators to restrict Audit
Log viewing rights based on the :ref:`Roles <auth-roles>` belonging to
a NetEye user and their groups. By default, users can only see their
own changes. If an administrator adds the role ``Allow viewing of all
logs in Activity Log``, then that user can see changes made by all
users.

The Audit Log View
~~~~~~~~~~~~~~~~~~

The main Audit Log View displays a table where each row is a change to
an object. Changes are grouped by the day of the change, and divided
into pages.

The controls above the table allow you to quickly navigate between the
pages and to filter changes according to:

* Changes made by the current user (“My changes”)
* The user account that made that change (if the viewer is an
  administrative user)
* The module that the change was made in
* The date on which the change was made (via the “Select date” date
  picker widget)

Clicking on the linked object in a row will take you to the
configuration panel for that object. Clicking anywhere else on a row
will take you to the Difference View panel described below.

You can configure the number of rows displayed per page by changing the
“Default page size” option under the **My Account** tab available from
the “gear” settings icon at the bottom left of the browser window.

The Difference View Panel
~~~~~~~~~~~~~~~~~~~~~~~~~

The Difference View panel shows a brief summary of the recorded
information (author, date, action and checksum) and both the old and new
objects in a side-by-side diff format.

.. _auditlog_retention_policy:

The Audit Log Retention Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Audit Log Retention policy defines the amount o time Logs will be stored.
This should prevent the database from continuous increase with new entries
being added. By default the retention policy is set to 365 days.
You can change the parameter in :menuselection:`Configuration / Modules / Auditlog / Configuration`
or manually in the file :file:`/neteye/shared/icingaweb2/conf/modules/auditlog/config.ini`.
Setting Retention policy to 0 days will keep the logs stored for an infinite
period of time or until the parameter is set to a different value.
