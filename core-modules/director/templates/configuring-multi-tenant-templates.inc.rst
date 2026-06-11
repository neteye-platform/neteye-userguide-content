.. _templates-for-multitenancy:

Templates to support Multi-Tenancy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Data security is a major concern in multi-tenancy environments, where
sensitive information used in the monitoring configuration of one tenant
must not be visible to other tenants after a deployment.

This kind of isolation cannot be solved by the director-global zones because they
are not adequate for separation of sensitive information, due to the fact that their
contents are distributed among all master, satellites, and agent nodes. Multi-tenancy
environments must implement another strategy which is to create a host template for
each zone owned by the tenant.

With this strategy, the host template data will be sealed inside the only zone owned
by the tenant and will not be visible to other tenants.

Please refer to the :ref:`How To <how_to-configure-monitoring-for-multi-tenancy>`
section that explains how to create a template for each zone to support the
multi-tenancy.


.. _how_to-configure-monitoring-for-multi-tenancy:

Template Configuration
~~~~~~~~~~~~~~~~~~~~~~

To create host template to support multi-tenancy, please go to the
:menuselection:`Icinga Director / Host objects / Host Templates`, click on the “+”
icon and fill the required fields along with the ``Cluster Zone`` information which
should be an ``Icinga Zone`` belongs to a tenant:

.. figure::  /core-modules/director/img/host-template-with-cluster-zone.png
   :alt: Creating a host template with Cluster Zone

   Creating a host template with Cluster Zone

Finally, click on “Add” to store the template in working memory. You must deploy the
template to push the changes in your monitoring environment.

.. warning:: No sensitive data must be stored on service templates. Please use the host templates for this purpose.

When there is more than one Icinga Zone for a single tenant, you will need to create duplicate
host templates, which are identical except for the ``Cluster Zone`` value.

Let's understand it with an example:

Suppose in a multi-tenancy environment, one of the tenant (i.e., ``tenantA``) has 3
satellites ``Sat_CloudA``, ``Sat_CloudAA`` and ``Sat_Internal`` and they are in the following
zones:

* Zone ``cloud`` = ``Sat_CloudA`` and ``Sat_CloudAA``
* Zone ``local`` = ``Sat_Internal``

Now, let's assume they all are checking the same service which requires the same credentials.
In that case, you need 2 identical host templates i.e., ``cloud-template`` and ``local-template``
except for ``Cluster Zone`` value which will be ``cloud`` for ``cloud-template`` and
``local`` for ``local-template``.

This additional configuration is required, because there is no way for the director to deploy a
host template that contains sensitive information in both zones (``cloud`` and ``local``) while
maintaining data secrecy.

.. note:: You don't have to touch the service templates since they must not contain sensitive data.
