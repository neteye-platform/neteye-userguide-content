

NetEye includes the ntopng software to allow for inspection of
networks flows. The module can be accessed using :ref:`Single Sign On <neteye-sso>`.

The ntopng UI can be reached by clicking the menu item
on the left-hand side. Depending on the roles of the users accessing the
module, available options and features may vary. Please
check :ref:`ntop configuration section <monitor-network-advanced>`
for more details about the permissions.

On NetEye, both ntopng and nProbe are running, with the latter being in
*Collector Mode*, i.e. it only collects flows sent to the 6363 port and
sends them to ntopng. Flows are collected by nProbe from any capable
network device (including, but not limited to, switches, servers,
printers, workstations) that can be reached within the local networks
accessible by NetEye.

Collected flows are sent to ntopng as ZMQ streams and processed; if
additional nProbes are installed on the local network, they can
as well be configured to send their flows to ntopng.

The realtime traffic information on the currently active flows can be
visualized by clicking on the :menuselection:`Flows / Live` entry in the sidebar.
The flows recorded in the past can be accessed from the
:menuselection:`Flows / Historical` menu entry.
ntopng stores historical flows and alerts thanks to an
integration with ClickHouse (an high-performance SQL database).

.. seealso:: The official documentation of
   `ntopng <https://www.ntop.org/guides/ntopng/index.html>`__,
   `nProbe <https://www.ntop.org/guides/nprobe/index.html>`__,
   `ClickHouse <https://clickhouse.com/docs/en/home>`__ contains more
   information about their architecture.

.. figure:: /feature-modules/ntopng/img/neteye-ntopng-schema.png
   :alt: NetEye - ntopng schema

   NetEye - ntopng architecture
