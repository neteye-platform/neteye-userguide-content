.. _ldap-import-configuration:

LDAP/AD Import Source configuration
```````````````````````````````````

The LDAP/AD interface allows you to import hosts and users directly from
a directory configured for the Lightweight Directory Access Protocol,
such as Active Directory.

The documentation below assumes that you are already familiar with
:ref:`importing and synchronization in Director <automating-imports>`.

Before creating an LDAP import source, you will need to configure a
*Resource* representing the connection to the LDAP server. Resources
have multiple purposes:

* :ref:`Authentication of users <roles-users-permissions>`
* Import of LDAP groups
* Import of hosts
* Import of users for notifications

A resource is created once for each external data source, and then
reused for each functionality it has. Some resource types are:

* Local database / file
* LDAP
* An SSH identity

In general, you will need to set up a resource for import when you
need to know its access methods in order to connect to it. For LDAP,
you will need the host, port, protocol, user name, password, and base
DN. To create a resource for your LDAP server, go to **Configuration >
Application > Resources** as shown in
:numref:`figure-add-ldap-resources`.

.. _figure-add-ldap-resources:

.. figure::  /core-modules/director/img/ldap-ad-import02.png
   :alt: Adding LDAP resource

   Adding LDAP  server as a resource.

Select the “Create a New Resource” action, which will display the “New
Resource” panel. Enter the values for your organization (an example is
shown in :numref:`figure-configure-connection-details`), then validate
and save the configuration with the buttons below the form. Your new
resource should now appear in the list at the left.

.. _figure-configure-connection-details:

.. figure::  /core-modules/director/img/ldap-ad-import07.png
   :alt: Configure connection details

   Configuring the vCenter connection details.

To create a new LDAP import source using the new resource, go to
**Director > Import data sources**, click on the “Add” action, then
enter a name and description for this import source. For “Source Type”,
choose the “Ldap” option.

As soon as you've chosen the Source Type, the form will expand
(:numref:`figure-configure-import-details`), asking you for more
details. Specify values for:

* The object key (key column name)
* The resource you created above
* The DC and an optional Organizational Unit from where to fetch the
  objects
* The type of object to create in NetEye (typically “computer”, “user”
  or “group”)
* An LDAP filter where you can restrict the results, for instance:
  * To exclude non-computer types
  * To exclude disabled elements
  * With a RegEx to filter for specific DNS host names

* A list of all LDAP fields to import in the “Properties” box, with
  each field name separated by a comma

:numref:`figure-configure-import-details` shows an example LDAP import
configuration. Finally, press the “Add” button.

.. _figure-configure-import-details:

.. figure::  /core-modules/director/img/ldap-ad-import09.png
   :alt: Configure the import details

   Configuring the LDAP import configuration details.

Your new import source should now appear in the list to the left, and
you can now perform all of the actions associated with importation as
described in the :ref:`section on automation <automating-imports>`.

.. image is blurred, it should be replaced at some point in the future
   including previewing the import as shown in
   :numref:`figure-ldap-import-preview`.

   .. _figure-ldap-import-preview:

   .. figure::  /core-modules/director/img/ldap-ad-import10.png
      :alt: The resulting LDAP import preview

      The resulting LDAP import preview

You will also need to define a **Synchronization Rule** for your new
LDAP import source. This will allow you to create helpful
:ref:`property modifiers <property-modifiers>` that can change the
original fields in a regular way, for instance:

* Resolve host names from IP addresses
* Check if a computer is disabled
* Standardize upper and lower case
* Flag workstations or domain controllers
