.. _kubernetes_networking_requirements:

Kubernetes Networking
---------------------

Since |ne| leverages Kubernetes for its container orchestration, it is mandatory to ensure that the networking
requirements are met.

For a more detailed discussion of the CIDR roles and how they interact with the current |ne| networking architecture,
please refer to the :ref:`Kubernetes Networking Architecture <kubernetes_networking_architecture>` section.

Summary of the Kubernetes networking
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When installing |ne|, define three CIDRs in :file:`/etc/neteye-environment.yaml`. These are Kubernetes address spaces,
not three additional physical networks. In particular, defining a CIDR does not create a VLAN, reserve addresses from
DHCP, or require the corresponding number of physical switch ports.

.. list-table:: Kubernetes address spaces
   :header-rows: 1
   :widths: 22 23 30 25

   * - Setting
     - Default
     - Purpose
     - Visible outside the |ne| cluster
   * - ``pod_cidr``
     - ``10.42.0.0/16``
     - Addresses assigned to Pods
     - No
   * - ``svc_cidr``
     - ``10.43.0.0/16``
     - Stable, virtual addresses for Kubernetes Services
     - No
   * - ``service_loadbalancer_cidr``
     - ``10.44.0.0/24``
     - Virtual IPs through which selected workloads are reached
     - Yes, but only from the trusted network zone

The Pod Network is the address space used by application and platform Pods. A Pod receives one address from the
per-node portion of ``pod_cidr`` for its lifetime. The Service Network is different: a Service IP is a stable virtual
frontend. Cilium redirects traffic sent to the Service IP to one of the current Pod backends, so a Service IP is not an
address assigned to a physical network interface.

The Service Load Balancer Network is used only for Services that intentionally accept traffic from outside the
Kubernetes cluster, such as a Gateway API listener. In a cluster deployment, Cilium assigns a virtual IP from this range
and announces it on the selected trusted Layer-2 network. It is the only one of the three ranges that nodes will reach
for communication with Kubernetes workloads. All traffic coming from clients are still being proxied through the
existing infrastructure (e.g. `httpd`, `nginx`, etc.) and then forwarded to the Load Balancer virtual IPs.

.. note::
   No switch, VLAN, DHCP, or router configuration is required for the Pod or Service CIDR. They are logical cluster
   address spaces managed on the nodes. The existing external-network configuration therefore remains unchanged for Pod
   and Service traffic. It is not necessary to create a separate VLAN for the Pod or Service CIDR because those ranges
   are not routed outside the cluster.

Choosing the CIDRs
~~~~~~~~~~~~~~~~~~

Users can choose any private CIDR (RFC 1918) for the three required ranges. It is recommended to choose ranges that do
not overlap with any other network intended to be reachable from the |ne| installation. This is because routing table
entries for the Pod and Service CIDRs are automatically added to the nodes, so any traffic sent to those ranges will be
routed to the Kubernetes workloads.

.. note::
   If future connectivity requires to route traffic between the |ne| installation and a client which happens to be in
   the same Pod or Service CIDR, you could exploit Satellites to monitor those hosts in the overlapping range.

For particular usecases, where no CIDRs are available, it is possible to use some of the IPv4 reserved ranges which are
explicitly prohibited for routing on the public Internet, such as the one specified in the RFC 6598 (Carrier-Grade NAT,
defined to be `100.64.0.0/10`). However, this is not recommended and should be avoided if possible as these ranges are
not intended for general use and may cause issues with certain applications and services, or could be requalified for
public use in the future.


CIDR reuse and overlap with other kubernetes clusters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Pod and Service ranges can be reused by separate Kubernetes clusters as VXLAN encapsulation ensures that the traffic is
isolated. However, the Service Load Balancer range must be unique and do not overlap a network reachable from either
cluster. This is common for isolated sites because these three ranges do not need to be globally routable.

Sizing the CIDRs
~~~~~~~~~~~~~~~~

Kubernetes network CIDRs cannot be enlarged after installation. Increasing an undersized range requires recreating the
Kubernetes cluster and migrating its workloads and configuration. This is a disruptive, time-consuming operation; choose
the CIDRs carefully before installation. If you are unsure about the size of the CIDRs or your environment is expected
to grow, and your IPAM situation allows it, choose a larger range than you think you will need.

Choosing the Pod CIDR
^^^^^^^^^^^^^^^^^^^^^^

Choose the Pod CIDR primarily according to the maximum number of Kubernetes nodes the cluster may contain, including
spare capacity for future expansion. Kubernetes allocates a separate ``\24`` portion of the Pod CIDR to each node, so
adding nodes consumes additional address space even when the current number of Pods is low.

Choose a range with sufficient growth headroom. If the Pod CIDR is exhausted, a new node cannot obtain a Pod address
range and cannot provide Kubernetes workloads.

The following table shows the maximum number of nodes that can be supported by different Pod CIDR sizes:

.. list-table:: Maximum number of nodes for different Pod CIDR sizes
   :header-rows: 1
   :widths: 22 23

   * - Pod CIDR size
     - Maximum number of nodes
   * - ``/16``
     - 256
   * - ``/17``
     - 128
   * - ``/18``
     - 64
   * - ``/19``
     - 32
   * - ``/20``
     - 16
   * - ``/21``
     - 8
   * - ``/22``
     - 4

Choosing the Service CIDR
^^^^^^^^^^^^^^^^^^^^^^^^^

About the Service CIDR, choose a range that can accommodate at least 512 Services for smaller installations, and at
least 1024 Services for larger installations. Each Service consumes one address from the Service CIDR, so the number of
Services is limited only by the size of the Service CIDR.

The following table shows the maximum number of Services that can be supported by different Service CIDR sizes:

.. list-table:: Maximum number of Services for different Service CIDR sizes
   :header-rows: 1
   :widths: 22 23

   * - Service CIDR size
     - Maximum number of Services
   * - ``/19``
     - 8192
   * - ``/20``
     - 4096
   * - ``/21``
     - 2048
   * - ``/22``
     - 1024
   * - ``/23``
     - 512

Choosing the Service Load Balancer CIDR
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This network is the least demanding in terms of address space. It is used only for Services that intentionally accept
traffic from outside the Kubernetes cluster, such as a Gateway API listener. A ``/24`` network is sufficient for most if
not all installations, as it can accommodate between 128 different |ne| nodes and 128 different Load Balancer Services.
Furthermore, changing the Service Load Balancer CIDR after installation is possible, although not implemented by any
|ne| command, requiring manual intervention and careful planning.
