
.. _active-monitoring:

.. _monitoring-environment:


Monitoring Environment
----------------------

NetEye builds around Icinga and Icinga Web 2, and so the monitoring environment
is organized according to **Roles** and **Zones**.

In Icinga2 you do not communicate directly with individual
monitored computers, but instead commands are passed along an encrypted
set of paths defined according to their **Role**:

-  **Master:** The node which sends your monitoring configurations to
   monitored objects, controlled by **IcingaWeb2** and **Director**.
-  **Satellite:** A node which forwards configurations from the master
   to each client in its zone.
-  **Client:** A node which receives configurations from a satellite,
   implements them, runs checks, and reports back the results.

**Zones** are logical collections of clients that should receive similar
configuration sets, headed by a **Satellite**.

**Endpoints** are nodes which are members of a zone, and are
characterized mainly by their IP address for implementing secure
communication.

The distributed monitoring environment is configured within the Director
Module. You can configure Zones and Endpoints by using the
**Infrastructure** menu :menuselection:`Menu --> Icinga Director -->
Activity log --> Infrastructure` within Director as in
:numref:`infrastructure-fig`.

.. _infrastructure-fig:

.. figure::  /core-modules/director/img/infrastructure-menu.png
   :alt: The infrastructure menu in Director

   The infrastructure menu in Director

The **Zones** panel allows you to create new **Master** and
**Satellite** nodes, as shown in Figure 2. Here you can add, modify,
and preview zones.

.. _zones-fig:

.. figure::  /core-modules/director/img/zones.png
   :alt: The Zones management panel

   The Zones management panel

Similarly, the **endpoints** panel allows you to create, modify and
preview endpoints, as in Figure 3.

.. _endpoints-fig:

.. figure::  /core-modules/director/img/endpoints.png
   :alt: The Endpoints management panel

   The Endpoints management panel

.. rubric:: Icinga2 Packages

In order to use all the functionalities of NetEye 4, follow our :ref:`Icinga2 Packages <icinga2-packages>` guide
to get icinga2 packages for different OS/distributions via the NetEye repositories for Icinga2 agent installation.
