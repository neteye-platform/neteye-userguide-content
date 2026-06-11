
Custom Dashboards for Hosts and Services
````````````````````````````````````````

When you need to go beyond having a single, default dashboard for every
host/service, it's time to make a customized dashboard for those hosts/services that need
them.

The following steps will guide you through the creation of a custom link that
will point to the customized dashboard. The link will be visible in the monitored object
**Actions** section.

.. figure:: /core-modules/director/objects/img/custom-analytics-dashboard-link.png
   :alt: Director - Custom analytics dashboard link

   Custom analytics dashboard link

If you need to create a custom dashboard for a particular service/host,
you can set the parameter in Director's service/host configuration. The
custom dashboard should be specified as *dataSource/dashboardName*.

.. _figure-itoa-hook-conf:

.. figure:: /core-modules/director/objects/img/host-form-with-custom-properties.png
   :alt: ITOA hook configuration

   ITOA hook configuration

The following steps show how you can create just such a custom dashboard
field (for a service, follow the procedure below while starting from a
service configuration rather than from a host).

First, click on **Director** in the left navigation sidebar, scroll to
the bottom of the page, and select "Define Data Field" (Figure 2).

.. _figure-director-data-field:

.. figure:: /core-modules/director/objects/img/data-field.png
   :alt: Director - Define Data Field

   Director Data Field

Now click "Add" and create a field with the field name set to
*custom\_analytics\_dashboard*, then enter a caption of your choice
(like "Custom Dashboard" in Figure 3).

.. _figure-custom-itoa-field:

.. figure:: /core-modules/director/objects/img/custom-analytics-dashboard-field.png
   :alt: Director - Custom ITOA Dashboard Field

   Custom ITOA Dashboard Field

Once you add the new data field, you should add it to a service/host
template. Open an existing template or create a new one (the same
procedure works both for hosts and services). Click on the **Fields**
tab, select the *custom\_analytics\_dashboard* field in the drop down
box, and add that field to the template. (Do not set it as mandatory,
because not all hosts/services will have or need a custom ITOA
dashboard.)

.. _figure-custom-template-field:

.. figure:: /core-modules/director/objects/img/add-custom-field-to-template.png
   :alt: Director - AddCustomFieldToTemplate

   Add Custom Field To Template

Lastly, deploy the final configuration. Now you can create a new
host/service starting from the modified template and add a custom ITOA
dashboard to it.
