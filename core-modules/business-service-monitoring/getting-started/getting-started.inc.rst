
To view or configure Business Processes in NetEye 4, click on
“Business Service Monitoring” in the left navigation bar. If you haven’t yet
configured any Business Processes yet, you will see a new Dashboard like
the one in :numref:`figure-bp-empty-dashboard` with two options:
“Create” and “Upload”.

.. _figure-bp-empty-dashboard:

.. figure:: /core-modules/business-service-monitoring/img/welcome-page.png

   The empty Business Service Monitoring Overview dashboard.

Configure a new Business Process
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create a new Business Process configuration, click on the “Create”
button. You should see a new dashboard panel
(:numref:`figure-bp-creation-panel`) that appears to the right of the
Business Service Monitoring Overview dashboard.

You will need to fill in the following fields to create the new Business
Process:

-  **ID:** The Business Process definition will be
   stored with this name. This is going to be used when referencing this
   process in URLs and in Check Commands.

-  **Display Name:** You might optionally want to provide an additional title.
   In that case the title is shown in the GUI, while the name is still
   used as a reference. The title will default to the name.

-  **Description:** Provide a short description explaining within
   100-150 character what this configuration provides. This will be
   shown on the Dashboard.

-  **Backend:** Icinga Web 2 currently uses only one Monitoring Backend,
   but in theory you could configure multiple ones. They won’t be usable
   in a meaningful way at the time of this writing. Still, you might
   want to use a different backend as a data provider for your Business
   Process.

   .. hint:: Usually this should not be changed.

-  **State Type:** You can decide whether ``SOFT`` or ``HARD`` states
   should be used as a basis for calculating the state of a Business
   Process definition.

-  **Add to menu:** Business Process configurations can be linked to the
   Icinga Web 2 left navigation bar. Only the first five configurations
   a user is allowed to see will be shown there.

You can also apply additional restrictions and specify **Allowed Users**, **Allowed Groups**
and **Allowed Roles**.

.. _figure-bp-creation-panel:

.. figure:: /core-modules/business-service-monitoring/img/create-business-process.png

   The new Business Process creation panel.

Store Business Process configuration
````````````````````````````````````

Once you are done, click ``Add`` to store your new (still empty)
Business Process configuration. The panel will show an empty “Add” tile
as in :numref:`figure-bp-created`, ready for you to add new Business Processes.

.. _figure-bp-created:
.. figure:: /core-modules/business-service-monitoring/img/new-bp-added.png

   The new Business Process creation panel.

From here you can now add as many deeply nested Business Processes as
you want.
