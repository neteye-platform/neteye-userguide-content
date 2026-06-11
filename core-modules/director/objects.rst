.. _objects:

Monitored Objects
-----------------

Once you have created templates for hosts, commands and services, you
can begin to create instances of monitored objects that inherit from
those templates. The guide below shows how to do this manually. In
addition, hosts can be imported using :ref:`automated discovery
methods <automating-imports>`, while services and commands can be
drawn from template libraries.

.. include:: objects/new-hosts-services.inc.rst
.. include:: objects/assign-services-hosts.inc.rst
.. include:: objects/add-custom-perf-dashboards.inc.rst
.. include:: objects/custom-dashboards-hosts-and-services.inc.rst
.. include:: objects/icinga2-packages.inc.rst
