.. _system-installation:

.. _install_neteye:

System Installation
===================

In this section you'll find guidelines to download, install, and set up
|ne| in different environments: as a Single Node or as a Cluster, and
on Satellites if necessary.

|ne| 4 is available as an ISO image. Please check section
:ref:`acquire-iso` for download instructions. The remainder of this
section consists of installation directions and is organized as
follows.

Section :ref:`neteye_hypervisors` guides you in the installation of
|ne| ISO image in the most popular virtualisation environments;
Section :ref:`single-node-install` provides the basic configuration
for Single Node and Satellites; Section :ref:`cluster-node-install`
gives instructions for NetEye Clusters and finally
:ref:`satellite-node-install` contains directions specific for
Satellite Nodes.

If your NetEye Node does not have direct access to Internet and
needs instead to pass through a proxy to reach the Internet, then you need
to configure the software running on NetEye to pass through this proxy,
as explained in Section :ref:`nodes_behind_proxy`.

If you need however to set up a reverse proxy in front of NetEye, please refer to this how-to:
:ref:`How to setup a reverse proxy <howto-networking-proxy>`


.. toctree::
   :maxdepth: 1

   system-installation/acquire-iso-image.rst
   system-installation/install-iso-image.rst
   system-installation/single-node-and-satellites.rst
   system-installation/cluster.rst
   system-installation/tenants-config.rst
   system-installation/satellites-only.rst
   system-installation/nodes-behind-proxy.rst
