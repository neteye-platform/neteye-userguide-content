
.. rubric:: User Permissions


Icinga Cube module checks restrictions set in the monitoring module and
only shows those monitored objects that the user is allowed to view
(this includes counts and states). Blacklist properties also restrict
what can be viewed. Restrictions applied to other modules will have no
effect.

.. rubric:: Getting Started


To use Cube in NetEye 4, click on the "Reporting" section in the
left-side navigation bar, and then the "Cube" subsection.

Note that if you haven't yet configured any custom properties for
monitored objects, you will see an empty dashboard.

As an example, consider the custom properties "Datacenter" and
"Operating System" that have been configured with values on a set of
monitored objects:

.. figure:: /core-modules/reporting/aggregated-view/img/host-custom-properties.png
   :alt: Host Custom Properties

   Host Custom Properties

You can view the resulting Cube as shown in Figure 1, where each box
shows the worst state and number of hosts for each discrete value of
"datacenter". You can then further drill down and rotate the cube by
adding and reprioritizing parameters (dimensions) in the dashboard.
