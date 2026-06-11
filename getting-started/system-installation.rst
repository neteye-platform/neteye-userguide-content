.. _system-installation:

.. _install_neteye:

System Installation
===================

In this section you'll find guidelines to download, install, and set up
|ne| in different environments: as a Single Node or as a Cluster, and
on Satellites if necessary.

|ne| 4 is available as an ISO image. Please check the section
:ref:`acquire-iso` for download instructions. The remainder of this
section consists of installation directions and is organized as
follows.

Section :ref:`neteye_hypervisors` guides you through the installation of
|ne| ISO image in the most popular virtualisation environments;
Section :ref:`single-node-install` provides the basic configuration
for Single Node and Satellites; Section :ref:`cluster-node-install`
gives instructions for |ne| Clusters, and finally
:ref:`satellite-node-install` contains directions specific for
Satellite Nodes.

If your |ne| Node does not have direct access to Internet and
instead needs to pass through a proxy to reach the Internet, then you need
to configure the software running on |ne| to pass through this proxy,
as explained in Section :ref:`nodes_behind_proxy`.

If however you need to set up a reverse proxy in front of |ne|, please refer to this how-to:
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
