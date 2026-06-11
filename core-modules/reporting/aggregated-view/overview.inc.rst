Cube
~~~~

Icinga Cube shows statistics and state for hosts and services
grouped by the custom properties that they have been configured with.
The subsets of monitored objects are then displayed in up to three
dimensions for a quick overview to show the differences between them.

It is especially helpful in large environments, and can help give quick
answers to questions like:

 * Which project uses how many servers per environment at which location/site?
 * How many of those are used in production?
 * Which operating system is used for which project and in which environment?
 * Are we still using Debian Lenny?
 * Do we have applications where the operating systems used differ in staging and production?
 * Which project uses which operating system version for which application?
 * Which projects have homogeneous environments?
 * Which projects are at a consistent patch level?
 * How many RHEL 6 variants (6.1, 6.2, 6.3...) do we use?
 * Who is running the oldest ones? In production?
 * Which projects are still using physical servers in which environments?

You can ask as many questions as you have combinations of configured
custom properties. The results are displayed with as many boxes as there
are values of the highest priority custom property.

.. figure:: /core-modules/reporting/aggregated-view/img/cube-dashboard.png
   :alt: Cube Dashboard

   Cube Dashboard
