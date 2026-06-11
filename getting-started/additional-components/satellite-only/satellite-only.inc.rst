.. _satellites-nodes-only:

Satellites Nodes only
---------------------

In order to install additional components on |NE| Satellites, the |NE| component should be
correctly installed on the Master following the :ref:`installation procedure <neteye-modules_installation>`.

Single Tenant
~~~~~~~~~~~~~

In a non-multitenant environment, all the installed |NE| components are already enabled. In order to apply
them on a Satellite follow the :ref:`neteye-satellite-conf` that will guide you through the
configuration or update of all satellites present in your system.

Multi Tenant
~~~~~~~~~~~~

In order to install additional |NE| components on satellites in a multi tenancy environment, the
following steps should be performed:

*   Enable the |NE| component for the desired Tenant using the ``--enable-module`` parameter of the
    ``neteye tenant config`` command. See :ref:`neteye-tenant` for more information.
*   Create or update the Satellites of that tenant in order to install the new activated |NE| component.
    Follow the :ref:`neteye-satellite-conf` section for more detailed information.
