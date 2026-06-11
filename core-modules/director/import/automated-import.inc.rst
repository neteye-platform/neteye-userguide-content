
Automated Import
````````````````

Automatically importing hosts, users and groups of users can greatly
speed up the process of setting up your monitoring environment if the
resources are already defined in an external source such as an
application with export capability (e.g., vSphere, LDAP) or an
accessible, structured file (e.g., a CSV file). You can view the
Icinga2 documentation `on importing and synchronizing
<https://www.icinga.com/docs/director/latest/doc/70-Import-and-Sync/>`__.

The following import capabilities (source types) are part of NetEye
Core:

-  **CoreApi:** Import from the Icinga2 API
-  **Sql:** Import rows from a structured database
-  **REST API:** Import rows from a REST API at an external URL
-  :ref:`LDAP <ldap-import-configuration>`\: Import from directories
   like Active Directory

Two other import source types are optional modules that can be enabled
or disabled from the **Configuration > Modules** page:

- :ref:`Import from files (fileshipper): <fileshipper-module>` Import
  from plain-text file formats like CSV
- :ref:`VMware <vsphere-import>`\: Import hosts/VMs from VMware/vSphere

You can import objects such as hosts or users (for notifications) by
selecting the appropriate field for import. For example in LDAP for the
field “Object class” you can select “computer”, “user” or “user group”.

The import process retrieves information from the external data
source, but by itself it will not permanently change existing objects
in NetEye such as hosts or users. To do this, you must also invoke a
separate :ref:`Synchronization Rule <sync-rules>` to integrate the
imported data into the existing monitoring configuration. This
integration could either be adding an entirely new host, or just
updating a field like the IP address.

For each synchronization rule you must decide how every :ref:`property
should map <import-source>` from the import source field to your
field in Neteye (e.g., from dnshostname to host_name). You can also
define different synchronization rules on the same import method so
that you can synchronize different properties at different times.

To trigger either the import or synchronization tasks, you must press
the corresponding button on their panels. Neteye also allows you to
schedule background tasks (:ref:`Jobs <automating-import-jobs>`) for
import and synchronization. You can thus create regular schedules for
importing hosts from external sources, for instance importing VMs from
vSphere every morning at 7:00AM, then synchronizing them with existing
hosts at 7:30AM. As with immediate import and synchronization, you
must define a separate job for each task.

To begin importing hosts into NetEye, select **Director > Import data
sources** as in :numref:`figure-automation-menu`.

.. _figure-automation-menu:

.. figure::  /core-modules/director/img/automate01.png
   :alt: The Automation menu

   The Automation menu section within Director.

.. _import-source:

The Import Source
`````````````````

The “Import source” panel containing all defined Imports will appear.
Click on the “Add” action to see the “Add import source” form (Figure
2). Enter a name that will be associated with this configuration in the
Import Source panel, add a brief description, and choose one of the
source types described above. The links above will take you to the
expanded description for each source type.

.. _figure-new-import-configuration:

.. figure::  /core-modules/director/img/vmware-import01.png
   :alt: Adding a new import configuration

   Adding a new import configuration for VMware/vSphere.

Once you have finished filling in the form, press the “Add” button to
validate the new import source configuration. If successful, you should
see the new import source added as a row to the “Import source” panel.
If you click on the new entry, you will see the additional tabs and
buttons in :numref:`figure-import-source-panels` with the following effects:

-  **Check for changes**: This button checks to see whether an import is
   necessary, i.e. whether anything new would be added.
-  **Trigger Import Run**: Make the importable data ready for
   synchronization.
-  **Modify**: This panel allows you to edit the original parameters of
   this import source.
-  **Modifiers**: Add or edit the *property modifiers*, described in the
   section below.
-  **History**: View the date and number of entries of previous import
   runs.
-  **Preview**: See a preview of the hosts or users that will be
   imported, along with the effects of any property modifiers on
   imported values.

.. _figure-import-source-panels:

.. figure::  /core-modules/director/img/vmware-import05.png
   :alt: Import source panels

   Import source panels

**Figure 3:** Additional tabbed panels and actions for the newly defined
import source.

.. _property-modifiers:

Property Modifiers
``````````````````

Properties are the named fields that should be fetched for each object
(row) from the data source. One field (column) must be designated as the
key indexing column (**key column name**) for that data source, and its
values (e.g., host names) must be unique, as they are matched against
each other during the synchronization process to determine whether an
incoming object already exists in NetEye. For instance, if you are
importing hosts, the key indexing column should contain fully qualified
domain names. If these values are not unique, the import will fail.

From the form you can select among these options:

* **Property:** The name of a field in the original import source that
  you want to modify.
* **Target Property:** If you put a new field name here, the modified
  value will go into this new field, while the original value will
  remain in the original **property** field. Otherwise, the property
  field will be mapped to itself.
* **Description:** A description that will appear in the property
  table below the form.
* **Modifier:** The property modifier that will be used to change the
  values. Once you have created and applied property modifiers, the
  preview panel will show you several example rows so that you can
  check that your modifiers work as you intended. :numref:`figure-property-modifier-panel` shows an
  example modifier that sets the imported user accounts to not expire.

.. _figure-property-modifier-panel:

.. figure::  /core-modules/director/img/property-modifier01.png
   :alt: Property modifier panel

   A property modifier to set imported user accounts as having no
   expiration.

These modifiers can be selected in the **Modifiers** drop down box as in
:numref:`figure-available-property-modifier`. Some of the more common modifiers are:

+------------------------+---------------+--------------+------------+
| Modifier               | Source        | Target       | Explanatio |
|                        | (Property)    |              | n/Example  |
+========================+===============+==============+============+
| Convert a latin1       | **guest.guest | **isRunning**| Change the |
| string to utf8         | State**       |              | text       |
|                        |               |              | encoding   |
+------------------------+---------------+--------------+------------+
| Get host by name (DNS  | **name**      | **address**  | Find the   |
| lookup)                |               |              | IP address |
|                        |               |              | automatica |
|                        |               |              | lly        |
+------------------------+---------------+--------------+------------+
| Lowercase              |**dnshostname**| (none)       | Convert    |
|                        |               |              | upper case |
|                        |               |              | letters to |
|                        |               |              | lower      |
+------------------------+---------------+--------------+------------+
| Regular expression     | **state**     | (none)       | /prod.*/i  |
| based replacement      |               |              |            |
+------------------------+---------------+--------------+------------+
| Bitmask match          | **userAccount | **is_ad_cont | 8192:      |
| (numeric)              | Control**     | roller**     | SERVER_TRU |
|                        |               |              | ST_ACCOUNT |
+------------------------+---------------+--------------+------------+

.. note:: For a description of Active Directory Bitmasks, please `see
   this Microsoft documentation
   <https://support.microsoft.com/en-us/help/305144/how-to-use-the-useraccountcontrol-flags-to-manipulate-user-account-pro>`__.

.. _figure-available-property-modifier:

.. figure::  /core-modules/director/img/property-modifier02.png
   :alt: Available property modifiers

   Available property modifiers

Once you have created the new property modifier, it will appear under
the “Property” list at the bottom of the Modifiers panel (see
:numref:`figure-property-modifier-panel`).

.. Note:: Here you can also order the property modifiers. Every
   modifier that can be applied to its property will be applied, so if
   you have multiple modifiers for a single property then be aware
   that they will be applied in the order shown in the list. For
   instance, if you add two regex rules, the second (lower) rule will
   be applied to the results of the first (higher).

.. _sync-rules:

Synchronization Rules
`````````````````````

When rows of data are being imported, it is possible that the new
objects created from those rows will overlap with existing objects
already being monitored. In these cases, NetEye will make use of
Synchronization Rules to determine what to do with each object. You can
choose from among the following three synchronization strategies, known
as the **Update Policy**:

-  **Merge:** Integrate the fields one by one, where non-empty fields
   being imported win out over existing fields.
-  **Replace:** Accept the new, imported object over the existing one.
-  **Ignore:** Keep the existing object and do not import the new one.

In addition, you can combine any of the above with the option to
**Purge** existing objects of the same **Object Type** as you are
importing if they cannot be found in the import source.

Each synchronization rule should state :ref:`how every property should
map <import-source>` from the import source field to your field in
Neteye (e.g., dnshostname -> host_name).

To begin, go to **Director > Synchronize** from the main menu and
press the green “Add” action in the **Sync rule** panel in
:numref:`figure-existing-sync-rules`.

.. _figure-existing-sync-rules:

.. figure::  /core-modules/director/img/sync-rule01.png
   :alt: Existing synchronization rules

   The Sync Rule panel showing existing synchronization rules.

Now enter the desired information as in
:numref:`figure-choose-object-type`, including the name for this sync
rule that will distinguish it from others, a longer description, the
**Object type** for the objects that will be synchronized, an **Update
Policy** from the list above, whether to **Purge** existing objects,
and a **Filtering Expression**. This expression allows you to restrict
the imported objects that will be synchronized based on a logical
condition. The official Icinga2 documentation lists all operators that
can be `used to create a filter expression
<https://www.icinga.com/docs/icinga2/latest/doc/17-language-reference/#operators>`__.

.. _figure-choose-object-type:

.. figure::  /core-modules/director/img/sync-rule02.png
   :alt: Choosing the object type

   Choosing the **Object Type** for a synchronization rule.

Now press the “Add” action. You will be taken to the **Modify** panel
of the synchronization rule, which will allow you to change any
parameters should you wish to. You should also see an orange banner
(:numref:`figure-new-sync-rule`) that reminds you to define at least
one **Sync Property** before the synchronization rule will be usable.

.. _figure-new-sync-rule:

.. figure::  /core-modules/director/img/sync-rule04.png
   :alt: Adding a new sync rule

   Adding a new sync rule

**Figure 8:** Successfully adding a new synchronization rule.

The color of the banner is related to the status icon in the “Sync rule”
panel:

+-----------------------------------------+----------------------------+
| Icon                                    | Banner/Meaning             |
+=========================================+============================+
| Black question mark                     | This Sync Rule has never   |
|                                         | been run before.           |
+-----------------------------------------+----------------------------+
| Orange cycling arrows                   | There are changes waiting  |
|                                         | since the last time you    |
|                                         | ran this rule.             |
+-----------------------------------------+----------------------------+
| Green check                             | This Sync Rule was         |
|                                         | correctly run at the given |
|                                         | date.                      |
+-----------------------------------------+----------------------------+
| Red “X”                                 | This Sync Rule resulted in |
|                                         | an error at the given      |
|                                         | date.                      |
+-----------------------------------------+----------------------------+

Synchronization Rule Properties
```````````````````````````````

A **Sync Property** is a mapping from a field in the input source to a
field of a NetEye object. Separating the mapping from the sync rule
definition allows you to reuse mappings across multiple import types.

To add a sync property, click on the “Properties” tab
(:numref:`figure-add-sync-property`) and then on the “Add sync
property rule” action. (Existing sync properties are shown in a table
at the bottom of this panel, and you can edit or delete them by
clicking on their row in the table.)

.. _figure-add-sync-property:

.. figure::  /core-modules/director/img/sync-rule05.png
   :alt: Adding a sync property

   Adding a first sync property.

:numref:`figure-setting-import-source` shows the first step, adding a
**Source Name**, which is one of the Import sources you defined in
:numref:`figure-new-import-configuration`. If you have multiple
sources, then this drop down box will be divided automatically into
those sources that have been used in a synchronization rule versus
those that haven’t.

.. _figure-setting-import-source:

.. figure::  /core-modules/director/img/sync-rule06.png
   :alt: Setting the Import Source

   Setting the Import Source

Next, choose the destination field
(:numref:`figure-setting-destination-field`), which corresponds to the
field in NetEye where imported values will be stored. Destination
fields are the pre-defined special properties or object properties of
existing NetEye objects. Note that some destination field values like
custom variables will require you to fill in additional fields in the
form.

.. _figure-setting-destination-field:

.. figure::  /core-modules/director/img/sync-rule08.png
   :alt: Setting the Destination Field

   Setting the Destination Field

If you cannot find the appropriate destination field to map to,
consider creating a custom field in the :ref:`relevant Host Template
<templates-conf>`.

Finally, choose the source column
(:numref:`figure-setting-source-column`), which is the list of fields
found in the input source.

.. _figure-setting-source-column:

.. figure::  /core-modules/director/img/sync-rule09.png
   :alt: Setting the Source Column

   Setting the Source Column

.. note:: Remember that the key column name is used as the ID during
   the matching phase. The automatic sync rule does not allow you to
   directly add any custom expressions to it.

Once you have finished entering the sync properties for a
synchronization rule, you can return to the “Sync rule” tab to begin
the synchronization process. As in :numref:`figure-trigger-sync`, this
panel will give you details of the last time the synchronization rule
was run, and allow you to both check whether a new synchronization
will result in any changes, as well as to actually start the import by
triggering the synchronization rule manually.

.. _figure-trigger-sync:

.. figure::  /core-modules/director/img/sync-rule10.png
   :alt: Preparing to trigger synchronization

   Preparing to trigger synchronization with our new rule.

.. _automating-import-jobs:

Jobs
````

Both **Import Source** and **Sync Rules** have buttons
(:numref:`figure-import-source-panels`) that will let you perform
import and synchronization at any moment. In many cases, however, it
is better to schedule regular importation, i.e., to *automate* the
process. In this case you should create a **Job** that automatically
runs both import and synchronization at set intervals.

The “Jobs” panel is available from **Director > Jobs**. Clicking on
the “Add” action will take you to the “Add a new Job” panel
(:numref:`figure-chose-type-jobs`) Here you will see four types of
jobs, only two of which relate to importation and synchronization:

-  **Config:** Generate and eventually deploy an Icinga2 configuration.
-  **Housekeeping:** Cleans up Director’s database.
-  **Import:** Create a regularly scheduled import run.
-  **Sync:** Create a regularly scheduled synchronization run.

.. _figure-chose-type-jobs:

.. figure::  /core-modules/director/img/jobs01.png
   :alt: Choosing the type of job

   Choosing the type of job

Select either the **Import** or **Sync** type. The following fields are
common to both:

-  **Disabled:** Temporarily disable this job so you don’t have to
   delete it.
-  **Run interval:** The number of seconds until this job is run again.
-  **Job name:** The title of this job which will be displayed in the
   “Jobs” panel.

If you choose **Import**, you will see these additional fields:

-  **Import source:** The import to run, including the option to run all
   imports at once.
-  **Run import:** Whether to apply the results of the import (Yes), or
   just see the results (No).

If instead you choose **Sync**, you will see these other fields:

-  **Synchronization rule:** The sync rule to run, including the option
   to run all sync rules at once.
-  **Apply changes:** Whether to apply the results to your configuration
   (Yes), or just see the results (No).

.. _figure-filling-values:

.. figure::  /core-modules/director/img/jobs02.png
   :alt: Filling in the values for a sync job

   Filling in the values for a sync job

Once you press the green “Add” button, you will see the “Job” panel
which will summarize the recent activity of that job, and the “Config”
panel, which will let you change your job parameters.
