.. _asset-management-glpi:


Overview
--------

The Asset Management Module is an additional |NE| feature module that
allows to *keep an inventory* of a company's IT infrastructure.
NetEye 4 integrates a solution to provide this functionality: the server
and agent part of the Open Source Software `GLPI <http://glpi-project.org/>`__.

GLPI helps you plan and manage IT changes in an easy way, solving
problems efficiently, automating business processes and gaining control
over the IT infrastructure. It provides advanced features for
inventory, asset and mobile devices management.

The inventory feature of GLPI serves to collect assets from multiple devices
in the monitored IT environment and list them in a inventory. This
functionality is granted by the GLPI Agents which send all the collected
assets to the Server in the NetEye Master.

GLPI can be directly accessed from the NetEye GUI (within the Asset
Management menu) using :ref:`Single Sign On <neteye-sso>`, with suitable
permissions (see :ref:`Configuration Section <glpi-permissions>`).
Upon the first access from a user, that user will be created inside GLPI
with a profile that reflects the permissions defined in NetEye.

The official, *full documentation* for GLPI is available directly from
within its interface.

.. note:: If the user logs out from NetEye, its **active GLPI session** will
   be closed automatically and it will be redirected to the NetEye
   login page.

.. note:: The users will be not able to use the GLPI's **Marketplace feature**,
   because we disable it to guarantee the integrity of the NetEye ecosystem.
