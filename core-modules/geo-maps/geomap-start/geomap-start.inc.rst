.. _geomap-visualizer:

The NetEye Geo Map Visualizer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The NetEye **Geo Map** module allows you to overlay the position and status of hosts on
top of a customizable map based on `OpenStreetMap <https://www.openstreetmap.org/>`_.
You can access Geo Map by clicking on the “Geo Map” menu item to the left.

.. figure:: /core-modules/geo-maps/img/geomap-map.png
   :alt: A Geo Map map

A Geo Map map.

.. rubric:: Geo Map Features


Geo Map uses a standard map interface that allows you to zoom in and
out of your chosen map. On each map you can place map markers
corresponding to hosts or groups of hosts, where the :ref:`color of
the marker <geomap-map-viewer>` reflects the current status of
the monitored objects, adopting the same colors of the monitoring modules.
Alternatively, markers can be configured to display the status of a host’s
services rather than the host status itself.

When multiple markers are placed at the same location, e.g. a server
room, the marker will show the worst status of all hosts or services
included in that marker. Clicking on a marker will launch a panel
containing :ref:`information on all hosts <geomap-host-information>`
represented by that marker.

Hosts are assigned to map layers as elements of a host group. Each
layer can be enabled and disabled via the map interface. You can
optionally save any map :ref:`as a dashlet <neteye-dashlets>` on the
NetEye dashboard, including multiple dashlets of the same map each
having different layers enabled.

The Geo Map module is fully integrated with Lampo search and IcingaWeb2
Global search. With both search tools you can query for map names and
find results both for map configuration and the map itself.

By default it is integrated with **Audit Log** module, which means that
every change done in the module configuration is logged and tracked into
the **Audit Log** section (see Auditlog user guide). If the **Audit
Log** module is not installed this feature is disabled. Following, the
list of the tracked actions:

* **Map** create, modify and delete.
* **Layer** create, modify, delete and sorting.
* **Field** list modify and sorting.
* **Module** configuration modify.

The Geo Map module also allows you to add map plugins that are
compatible with the `Leaflet <https://leafletjs.com/>`_ JavaScript
library.
