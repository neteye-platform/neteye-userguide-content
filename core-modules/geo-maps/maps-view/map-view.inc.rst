.. _geomap-map-viewer:

Map Viewer
~~~~~~~~~~

When you click on the Geo Map menu item, you will see a list of the
available, defined maps (:numref:`figure-geomap-map-list`). Clicking
on a map entry will take you to the map itself.

.. _figure-geomap-map-list:

.. figure:: /core-modules/geo-maps/img/geomap-map-list.png
   :alt: List of available maps

   List of available maps.

On the map, clicking on a marker will show the monitoring view for the
host(s) associated with that marker in a panel to the right (see the
section on  :ref:`geomap-host-information` below.)

The pushpin icon at the top left will reposition the map at your
inferred location with a blue dot representing you, and the estimated
position error with a larger light blue circle
(:numref:`figure-geomap-localize`). You can enable and disable layers
with the checkbox panel at the top right.

.. _figure-geomap-localize:

.. figure:: /core-modules/geo-maps/img/geomap-localize-01.png
   :alt: geomap localization

   Viewing the map and localizing your position.

You can add a map whose properties you have configured to the dashboard
as a dashlet. Set the active map layers as desired, and then select “Add
To Dashboard” from the chevron menu next to the map panel’s title. The
URL with the active layers will be automatically filled in for you, and
you only need to fill in the dashlet’s title.

.. _geomap-host-information:

.. rubric:: The Host Information Page


The host information page contains data on the hosts located at a
location represented by a marker on the map. It opens as a panel on
the right side of the screen (:numref:`figure-geomap-host-info`) and
contains the following:

- **(Cluster) State:** The state of the host (or hosts) as determined
   by the :ref:`status calculation <geomap-configure-maps>`
-  **Common Host Information:** Information that is the same for all
   hosts at the marker, which typically includes at least the
   geographical coordinates. If optional fields are present, they follow
   the order determined in the Map Configurator
-  **Detailed Information:** A list of all hosts, whose fields contain
   values that differ from each other, sorted from worst to
   best status

.. note:: The rows on the host information table are clickable.  Once
   clicked, you will see either the host details page when *Status
   Calculation* in map configuration is set to *Host Status* or the host
   service list page if the same option is set to *Services worst state of
   all Hosts*.

.. _figure-geomap-host-info:

.. figure:: /core-modules/geo-maps/img/geomap-host-info-01.png
   :alt: The host information page

   The host information page.
