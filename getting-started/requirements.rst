
Requirements
============

This section lists all the requirements that must be satisfied to
install |ne| and is organized in these parts:

* :ref:`neteye-single-requirements` presents the requirements for the
  installation of a |ne| Single Node and Satellite Node, and for each
  Cluster Node. Moreover, supported hypervisors are also listed with
  some requirements

* :ref:`cluster-guidelines` is a conversational section that
  introduces and describes general guidelines and best practices that
  should be taken into account when designing a new cluster
  infrastructure

* :ref:`neteye-satellite-requirements` contains a list of requirements
  that need to be satisfied on the Satellite Nodes in addition to the
  ones described in :ref:`neteye-single-requirements`

* :ref:`neteye-ports-requirements` list all the TCP and UDP ports that
  should be opened to allow flawless functioning of a |ne|
  installation, separated into system ports and module-specific ports

* :ref:`additional-software-installation` explains the best practices on
  how the |ne| Administrator should manage additional
  software on the |ne| Nodes

.. toctree::
   :maxdepth: 1

   requirements/single-node.rst
   requirements/cluster.rst
   requirements/kubernetes-networking.rst
   requirements/satellite.rst
   requirements/tcp-udp-ports.rst
   requirements/additional-software.rst
