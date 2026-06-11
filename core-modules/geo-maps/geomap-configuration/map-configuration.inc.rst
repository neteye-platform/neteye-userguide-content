Configuring Geo Maps
~~~~~~~~~~~~~~~~~~~~

NetEye is pre-configured to use OpenStreetMap’s standard tile layer as a
map tile server. Should you desire to change to a different server that
is compatible with OpenStreetMap, go to **Configuration > Modules >
geomap** and find the field “Server Url for Tiles”. Change the URL
structure, making sure to keep the placeholder variables ({s}, {x},
etc.) in the correct positions in the new URL so that the appropriate
tiles will be retrieved from the new tile server.

The `OpenStreetMap.org URL format
<https://wiki.openstreetmap.org/wiki/Slippy_map_tilenames#Zoom_levels>`_
is the following:

.. csv-table:: Tile Map URL Parameters
   :header: "Var", "Explanation"

   "s", "A subdomain to get around limitations by browsers on the
   number of simultaneous HTTP connections to the tile server."
   "x", "The index of a map tile along the x-axis of the map under
   spherical Mercator projection."
   "y", "The index of a map tile from the North pole (0) to the South
   pole (2z - 1)."
   "z", "The zoom level, where 0 represents the entire world, 18 is
   the maximum zoom, and each level n in between represents a 2n by 2n
   zoom factor."


In the same panel, you can also configure the maximum radius in map
pixels that will determine which hosts are included in a cluster. You can also
select the default way the map should be open (*New Column*, *Same Column*,
*Single column* and *New Tab*).

.. rubric:: Entering Device Coordinates


For each device (host, router, etc.) that you wish to appear on the map,
you will have to enter its longitude and latitude in its configuration
within Director. For instance, for a host, navigate to **Director > Host
objects > Hosts**, then click on the desired host (optionally, you can
set the coordinates for a wide range of hosts/devices by setting them
for a Host Template).

Next, scroll down to and expand the “Custom properties” section. You
will see the field “Location (lat,lng)” next to a text field with a
globe icon below it. Clicking on that icon allows you to use a world
map to set the coordinates with the `MapDataType plugin
<https://github.com/nbuchwitz/icingaweb2-module-mapDatatype>`_ rather
than entering them by hand.

To use the plugin map, click on the pushpin and wait until the map has
changed to your position. (You may need to explicitly tell your browser
to allow location access.) Zoom out to ensure that the localization is
correct, and then left click on the exact position (to drag the map,
left click and hold the cursor while you move). Then click on the
“check” button below the map. The coordinates of the location you
selected will then automatically be entered in the location field.

If instead you prefer to enter the coordinates by hand, you can type
then directly into the text field, or disable the MapDataType plugin
with the following shell command:

::

   # icingacli module disable mapDatatype

.. _geomap-configure-maps:

.. rubric:: Configuring Maps


To create a new map, go to **Geo Map > Configurator > Maps
Configurator** and click on ``Add``. (To edit an existing map, just
click on the map’s tile instead.) Then in the ``Add Map`` panel that
appears, fill in the following values:

* **Name:** The name of this map that distinguishes it from other
  maps.
* **Description:** Add a short description that better explains the
  map.
* **Show counts in cluster installation:** If set to **yes**, the map
  will display all the monitored objects with the same geo-location
  under a **single cluster-icon**, displaying the number of
  “clusterized” objects.  Otherwise, a single marker will appear,
  showing the icon set for the upper layer visible on the map.
* **Show labels in detailed information:** If set to **yes**, the
  labels of the fields in the detailed information table will be displayed.
* **Prevent merging of cluster markers:** If set to **yes**, it prevents the
  markers belonging to different layers to be merged into a single cluster
  when zooming out.
* **Target:** this option allows the user to override the module configuration,
  and for the single map to open in *New Column*, *Same Column*,
  *Single column* and *New Tab*.
* **Map default view:** this section allows the user to define how the map should render.
  In particular, if the flag **Set custom values for map center and zoom** is set to **no**,
  the map will automatically set its zoom and center in order to fit all the markers
  in the details view. If the flag is set to **yes** instead, a sample map will appear.
  The user will just have to set the desired view, in order to fit inside the panel
  the portion he expects to see in the map details section. Geomap will automatically
  record the zoom level and the map center. These data that will be used
  during the rendering process of the map details.
* **Status calculation:** Selects the method that chooses how to set a
  map marker’s color

  * **Host status:** The color corresponding to the status of the host
    at that marker (or all hosts at that cluster of markers).
  * **Services worst state of all hosts:** The color corresponding to
    the worst status of any service over the host at that marker (or
    all hosts at that cluster of markers).

  Regardless of the chosen method, the worst status of a group of hosts or
  services is determined according to the following order, from worst to best:

  #. Unknown/Unreachable
  #. Unknown/Unreachable acknowledged
  #. Down/Critical
  #. Down/Critical acknowledged
  #. Warning
  #. Warning acknowledged
  #. Pending
  #. Up/Ok

There are two additional tabs on the right for creating layers that will
appear on the map and fields that will appear on the host information
page. These are described in the sections below.

.. rubric:: Configuring Layers


Each layer that appears on a map represents the location and status for
a set of hosts. Map Layers allow you to toggle the visibility of each
layer independently of the others, without having to change the
visibility of each host individually.

.. the three crossrefs below were broken also in current UG, because
   they refer to docs that is not there. I replaced them with nearest
   matches, let's defer the final fix when we'll review this part.

You can see the layers associated with a map by going to **Geo Map >
Configurator >** *Map* **> Map Layers**. Each layer corresponds to a
row in the table, consisting of the layer’s name and ordering
options. Layer ordering works on the *order of
processing* principle, where each layer is processed in turn, with
information from farther down the table overwriting any information
already written out earlier.

To add a new layer to the map, click on ``Add``. (To edit an existing map,
just click on the map’s name.) Then in the ``Add Layer`` panel that appears,
fill in the following values:

* **Layer Name:** The name of this layer to distinguish it from other
  layers.
* **Enabled by default:** Whether this layer is visible when you first
  load a new map.
* **Icon for markers:** A field to insert an SVG icon for markers (in
  an XML text representation).
* **Host Groups:** The hosts to include (via their host groups) in
  this layer. Use the `multi-select tool`
  to select host groups by moving them to the box on the right. You
  can include a host group in more than one layer if desired.

You can use custom marker icons, with a distinct icon for each layer. In
the “Icon for markers” field, insert the XML text for an SVG icon (SVG
is required so that the icon can change color reflecting the current
host status). You will need to edit the icon XML to make sure that it
doesn’t contain any width or height attributes that may cause the icon
to be too large or small. By default, the text area will include the XML
of a standard marker SVG icon when you initially create the layer.

Custom icons will only be shown either when there is a single host at
that point on the map, or when all hosts in a cluster have the same icon
type. Otherwise, the default map icon will be displayed to avoid showing
multiple icons at the same point.

.. note:: the host groups that are available in the left side of the
   multi-select tool are those that have at least one host configured
   in Director with valid coordinates for the “Location” property.


.. rubric:: Configuring Fields


To add fields that you would like to appear on a host’s information page
to a particular map, go to **Geo Map > Configurator > Map > Map
Fields**. Then choose the fields that you want to be displayed on the
host information page for that map.

To select fields, use the `multi-select tool` to move them to the box
on the right. You can also change the order of the fields you have
selected by clicking on that field and pressing the up or down
button. The order in which the fields appear here will be reflected in
the order they are shown on the host information page.

.. rubric:: User Permissions in Geo Map


A NetEye 4 administrator can set 3 types of Geo Map permissions for
users via Roles:

* **Map Admin:** Create, modify and view maps.
* **Map Editor:** Modify and view maps, where map names can be
  filtered by a regular expression set by a system admin (where no
  regex means all maps can be modified or viewed).
* **Map Viewer:** View maps, where map names can be filtered by a
  regular expression set by a system admin (where no regex means all
  maps can be modified or viewed).

To set the regex filters, a NetEye administrator can go to the Roles
page at **Configuration > Authentication** and fill in the
“geomap/filter/maps” field.
