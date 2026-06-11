
Assign Service Templates to Hosts
`````````````````````````````````

Once you have created host and service templates, and individual hosts
and services, you can assign service templates to hosts. (Only service
templates can be assigned, not simple services.) There are three
situations in which you can do this, and they are all performed in a
similar manner:

-  Assign a service template to a host
-  Assign a service template to a host template
-  Assign a service template to a service set

For instance, to assign a service template to a host, go to **Director >
Host objects > Hosts** and select the desired host. In the panel that
appears on the right (see Figure 1), select the **Services** tab, then
click on the “Add service” action and choose the desired service
template from the “Service” drop down menu.

.. _figure-assign-service-template:

.. figure::  /core-modules/director/img/assign-service01.png
   :alt: Adding a service template to a host

   Adding a service template to a host

Then under “Main properties”, choose the appropriate service or service
template in the drop down under **Service**.

To add a service template to a host template, follow the above
instructions after choosing a host template at **Director > Host objects
> Host Templates**.

For service sets, follow **Director > Services, Service Sets**, select
the desired service set, change to the **Services** tab, and click the
“Add service” action, choosing the service template from the “Imports”
drop down menu.
