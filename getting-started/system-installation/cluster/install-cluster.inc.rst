|ne| 4’s clustering service is based on the RedHat 8 High Availability
Clustering technologies:

* `Corosync <http://corosync.github.io/corosync/>`_: Provides group
  communication between a set of nodes, application restart upon
  failure, and a quorum system.
* `Pacemaker
  <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/assembly_overview-of-high-availability-configuring-and-managing-high-availability-clusters#con_pacemaker-overview-overview-of-high-availability>`_: Provides cluster
  management, lock management, and fencing.
* `DRBD <https://docs.linbit.com/docs/users-guide-9.0/>`_: Provides
  data redundancy by mirroring devices (hard drives, partitions,
  logical volumes, etc.) between hosts in real time.

Cluster resources are typically quartets consisting of an internal
floating IP, a DRBD device, a filesystem, and a (systemd) service.

Once you have installed clustering services according to the information
on this page, please turn to the :ref:`Cluster Architecture
page <neteye-cluster-architecture>` for more
information on configuration and how to update.

.. seealso::

   For more information about RedHat Cluster, check the `official
   RedHat's documentation
   <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_high_availability_clusters/index>`_
   on High Availability Clusters.


Prerequisites
~~~~~~~~~~~~~

A |ne| 4 cluster must consist of between 2 and 16 identical servers
(*"Nodes"*) running :abbr:`RHEL (Red Hat Enterprise Linux)` 8; each
node must satisfy the following requirements:

- Networking:

  - Bonding across NICs must be configured
  - A dedicated cluster network interface, named exactly the same on
    each node
  - One external static IP address which will serve as the external
    Cluster IP
  - One IP Address for each cluster node (i.e., N addresses)
  - One virtual (internal) subnet for internal floating service IPs
    (this subnet MUST NOT be reachable from any machine except cluster
    nodes, as it poses a security risk otherwise)
  - All nodes must know the internal IPs (Virtual IP) of all other
    nodes, which must be stored in file :file:`/etc/hosts`
  - All nodes must be reachable over the internal network
  - The Corporate Network's NIC must be in firewall zone **public**,
    while the Heartbeat Network's NIC must be in firewall zone
    **trusted**

- Storage:

  - At least one volume group with enough free storage to host all
    service DRBD devices defined in Services.conf

- In general, each node in a |ne| Cluster...

  - must have SSH keys generated for the ``root`` user
  - must store the SSH keys of all nodes in file
    :file:`/root/.ssh/authorized_keys`
  - needs Internet connectivity, including the ability to reach
    repositories of |witit| and Red Hat
  - must have the dnf group **neteye** installed
  - must have the tags set with the command :command:`neteye node tags set`.
    To know more about this command please refer to :ref:`neteye-node-tags-set`
  - must be subscribed with a valid Red Hat Enterprise Linux license.
    This can be done with the command :command:`neteye node register`.
    To know more about this command please refer to :ref:`neteye-node-register`
  - must have the latest operating system and **NetEye 4**
    updates installed
  - if a *virtual* Cluster Node, its RAM memory must be **completely
    reserved**
  - requirements for characters that can be used in the hostnames are
    the same for Single and Satellite Nodes and can
    be checked in the :ref:`installation procedure <ne-setup-part-one>`

.. seealso:: Section :ref:`cluster-configuration` contains more
   detailed requirements for |ne| cluster installation.

Installation Procedure
~~~~~~~~~~~~~~~~~~~~~~

The first step of a |ne| Cluster installation is to install the |ne|
ISO image, after which you need to follow, for each Node,
installation's :ref:`ne-setup-part-one`. Then, make sure to copy the
SSH key of each node on all the other Nodes'
:file:`/root/.ssh/authorized_keys` file. To accomplish this goal, you
can use this command on each Node:

.. code:: bash

   cluster# ssh-copy-id -i /root/.ssh/id_rsa.pub root@172.27.0.3

Repeat the command for each Node, replacing *172.27.0.3* with the IP
address of each of the other Nodes.

Once done, depending on the type of nodes you are installing in your
cluster, select either of the following procedures:
:ref:`basic-cluster-installation`,
:ref:`neteye-service-installation`, or :ref:`single-purpose-node`.

Once done, if your |ne| Cluster setup includes satellites, please
make sure to carry out the steps in section
:ref:`satellite-node-install` after each Satellite Node's
installation.

.. _basic-cluster-installation:

Basic Cluster Installation
``````````````````````````

This task consists of two steps:

#. Copy the cluster configuration ``json`` template from
   :file:`/usr/share/neteye/cluster/templates/ClusterSetup.conf.tpl` to
   :file:`/etc/neteye-cluster` and edit it to match your intended setup. You
   will be required to fill the following fields:

   .. csv-table::
      :header: "Key", "Type", "Description"

       "ClusterInterface", "str", "The name of the internal cluster network interface"
       "Hostname", "str", "Cluster's FQDN that will resolve to ClusterIp"
       "ClusterIp", "str", "Floating IP address reserved for the cluster"
       "ClusterCIDR", "int", "Netmask in CIDR notation (8-32)"
       "Nodes", "list", "List of ``Operative node`` (must be at least 2)"
       "VotingOnlyNode", "object", "(Optional) Definition of the ``Voting only node``"
       "ElasticOnlyNodes", "list", "(Optional) List of ``Elastic only nodes``"


   All the nodes specified in ``Nodes``, ``VotingOnlyNode`` and
   ``ElasticOnlyNodes`` must have all of the following fields:

   .. csv-table::
      :header: "Key", "Type", "Description"

       "addr", "str", "The internal ip address of the node"
       "hostname", "str", "Internal FQDN of the node"
       "hostname_ext", "str", "External FQDN of the node"
       "roles", "list", "List of roles assigned to the node. The complete list of the roles assignable to a node can be found in :file:`/usr/share/neteye/cluster/config_validators/roles.d/`"
       "id", "int", "An unique, progressive number (Note: ElasticOnlyNodes don't require this field)"



#. After setting up the cluster configuration in :file:`/etc/neteye-cluster`,
   run the command :ref:`neteye-config-cluster-check` to verify that the
   configuration is correct. This command will check that the configuration
   defined in the :file:`/etc/neteye-cluster` file is correct and that all
   the roles have a valid configuration in terms of node distribution.

   .. code:: bash

      cluster# neteye config cluster check

#. On one node of the cluster run the :command:`neteye feature-module add core` to prepare the service files for
   core cluster resources.

#. For any additional feature module that needs to be installed, run the :command:`neteye feature-module add <module_name>`
   to install the packages for the feature module and also prepare the service files for
   the cluster resources.

#. On the node where you run the feature module installations, review
   the templates for the cluster services that you can find in :file:`/etc/neteye-services.d/<module_name>/`
   and edit the options as needed. Please note that, for services needing an IP, a proposal
   for the IP address will be automatically generated based on the free IPs on the internal cluster subnet.
   No need to sync the files across nodes, as the cluster installation procedure will take care of it.

#. Run the cluster setup command :command:`neteye cluster install` to install
   the PCS basic cluster and its resources. In case of any
   issue which prevents the correct script execution you can run the same
   command again adding the option ``--force`` to override. This will destroy
   the existing cluster on the nodes.

   .. code:: bash

      cluster# neteye cluster install

   .. note:: Any not recognised option given to the
      :ref:`neteye-cluster-install` command will be passed to the internal
      Ansible installation command.


#. At this point, all cluster nodes must be online, hence, as last
   step, verify that the Cluster installation was completed
   successfully by running the command:

   .. code:: bash

      cluster# pcs status | grep -A1 -C1 Online

   This command returns something like:

   .. container:: codeblock

      .. code::

         Node List:
         * Online: [ my-neteye-01.example.com my-neteye-02.example.com ]

   If the installation includes also a **Voting-only Node**, check that it
   is online by running:

   .. code:: bash

      cluster# pcs quorum status

   The bottom part of the output is similar to the following snippet:

   .. container:: codeblock

      .. code::

         Membership information
         ----------------------
         Nodeid Votes Qdevice Name
         1   1  A,V,NMW my-neteye-01.example.com (local)
         2   1  A,V,NMW my-neteye-02.example.com
         0   1  Qdevice

   The *last line* shows that the Voting-only Node is correctly online.

   .. _cluster-fencing-configuration:

Cluster Fencing Configuration
`````````````````````````````

This section describes the procedures to configure, test, and manage the fence devices in a cluster.
Fencing is useful when a node is unresponsive and may still be accessing data.
The only way to be certain that your data is safe is to fence the node using STONITH. STONITH is an acronym
for "Shoot The Other Node In The Head" and it protects your data from being corrupted by rogue nodes or
concurrent access. Using STONITH, you can be certain that a node is truly offline before allowing the data
to be accessed from another node.

    .. seealso::

        For more complete general information on fencing and its importance in a Red Hat High Availability cluster,
        see `Fencing in a Red Hat High Availability Cluster`_.

        .. _Fencing in a Red Hat High Availability Cluster: https://access.redhat.com/solutions/15575



#. Initial Setup

    - Fencing can be enabled upon setting an environment variable.
      However, it is recommended to keep fencing disabled until it is configured properly:

      .. code::

          pcs property set stonith-enabled=false
          pcs stonith cleanup

    - Install ipmilan fence device on **each node**

      .. code::

          yum install fence-agents-ipmilan

    - Test that IDRAC interface is reachable on port 623 on **each node**

      .. code::

          nmap -sU -p623 10.255.6.106

    .. note:: Fencing on VMware Cluster
       In the case you're a installing a virtual cluster please keep in mind that
       a fencing device must be different from IPMI. To install a fence device
       on VMware Cluster apply the following command:

       .. code:: bash

          dnf install fence-agents-vmware-rest fence-agents-vmware-soap


#. IDRAC Configuration

    - Enable IPMI access to IDRAC: IDRAC Settings > Connectivity > Network > IPMI Settings

        * Enable IPMI Over LAN: Enable
        * Channel Privilege Level Limit: Administrator
        * Encryption Key*:  <mandatory random string, also 00000000>

    - Create a new user with username and password of your choice, Read-only privileges on console but administrative privileges on IPMI.
      (**IDRAC Settings > Users > Local Users > Add**)

        * User Role: Read Only
        * Login to IDRAC: enable

    - Advanced Settings

        * LAN Privilege Level: Administrator

    To test that the settings were properly applied to a news user you can check the status from NetEye machine

    .. code::

        ipmitool -I lanplus -H <IDRAC IP> -U <your_IPMI_username> -P <your_IPMI_password> -y <your_encryption_key> -v chassis status

#. PCS Configuration

    To obtain information about your fence device run:

    .. code::

        pcs stonith list
        pcs stonith describe fence_idrac

    **Create a fence device**

    The following instructions will help you create a fence device.

    .. code::

        pcs stonith create <fence_device_name> fence_idrac ip="<ip or fqdn>" pcmk_delay_base='5' lanplus="1" username="IPMI_username" password="IPMI_password" method="onoff" pcmk_host_list="<host_to_be_fenced>"

    Where:

    * fence_device_name: device name of your choice (e.g. idrac_node1)

    * fencing_agent: in this case fence_idrac, you can obtain this with pcs stonith list

    * ip: IDRAC IP or FQDN

    * pcmk_delay_base: by default is 0, must differ on nodes by 5 seconds or more, based on how fast iDRAC can initiate a shutdown

    * lanplus: set always at 1 otherwise it will not connect

    * username: IPMI username (created before)

    * password: IPMI password (created before)

    * password_script: an alternative to password, if available you should use this instead of plain password

    * method: usually you should ‘onoff’ if available otherwise restart is not guaranteed (power off/power on)

    * pcmk_host_list: list of host controlled by

    .. warning::

        In a 2-node cluster it may happen that both nodes are unable to communicate and both try to fence each other.
        This will cause a reboot of both nodes. To avoid this, set different pcmk_delay_base parameters
        for each fence device; this way one of the nodes will acquire more priority over the other.

        It is strongly suggested to set this parameter for EVERY cluster regardless of the number of its nodes.

    .. note::

        If possible use a **password_script** instead of **password**, as anybody with access to
        PCS can see the IPMI password. A password script is a simple bash script which performs an echo of
        the password and is also helpful to avoid escaping problems e.g.

        ``#!/bin/bash
        echo 'my_secret_psw'``

        and only root user has read privileges on it. (FYI ``chmod 500``)

        You must put this script on all nodes e.g. in ``/usr/local/bin``

    Example:

    .. code::

        pcs stonith create idrac_node1 fence_idrac ip="idrac-neteye01.intra.example.com" lanplus="1" username="neteye_fencing" password_script="/usr/local/bin/fencing_passwd.sh" method="onoff" pcmk_host_list="node01.neteyelocal" pcmk_delay_base='10'

    If your fence device has been properly configured running ``pcs status`` you should see the fencing device
    in status **Stopped** otherwise check in /var/log/messages.

    ``pcs stonith show <fence device>`` displays the current setup of device

    Now you have to create a fence device for each node of your cluster (remember to increase the delay)

    .. note::

        If you need to update a fence device properties, use the update command, e.g.:

        .. code::

            pcs stonith update <fence device> property=”value"

#. Only for 'onoff' method

    edit the power key on ``/etc/systemd/logind.conf``

    .. code::

        HandlePowerKey=ignore

    To do it programmatically:

    .. code::

        sed -i 's/#HandlePowerKey=poweroff/HandlePowerKey=ignore/g' /etc/systemd/logind.conf

#. Increase totem token timeout

    Increasing totem token timeout at least to 5 seconds will avoid unwanted fencing (default is 1s); on cluster
    with virtual nodes it should be set to 10. It is not recommended to set the timeout to more than 10 seconds.

    .. code::

        pcs cluster config update totem token=10000

    To check if the value has been updated:

    .. code::

        corosync-cmapctl | grep totem.token

    .. warning::

        Stonith acts after totem token expiration, therefore it may take also 30-40 seconds to fence a node

    .. seealso::

        https://access.redhat.com/solutions/221263


#. Testing

    To fence a device you can use the following command:

    .. code::

        pcs stonith fence <node1.neteyelocal>

    .. warning::

        The host will now be taken to a shutdown mode. Fencing should be tested on a node in standby.

#. Enable fencing

    To enable fencing set property to true

    .. code::

        pcs property set stonith-enabled=true
        pcs stonith cleanup

    .. warning::

        If fencing fails cluster freezes and resources will not be relocated on a different node.
        Always disable fencing during updates/upgrades.
        Disable fencing on virtual machines before shutting them down: it may happen that a fence device
        restarts a shutdown VM.
        A restart of a physical node may require several minutes so please be patient.


.. _neteye-service-installation:

|ne| Service Configuration
``````````````````````````

* Run the :ref:`neteye install <neteye-install>` script only once on any cluster
  node. This script is designed to handle the configuration of all nodes
  specified in the cluster configuration file found at
  :file:`/etc/neteye-cluster`.

  .. code:: bash

     cluster# neteye install

* Set up the Director field **API user** on slave nodes
  (:menuselection:`Director / Icinga Infrastructure / Endpoints`)

.. _single-purpose-node:

Single Purpose Nodes
~~~~~~~~~~~~~~~~~~~~

This section applies only if you have are going to setup a *Single
Purpose Node*, i.e., an Elastic-only or a |ne| Voting-only node.

Both **Elastic-only** and **Voting-only** nodes have the same
prerequisites and follow the same installation procedure as a standard
|ne| Cluster Node.

After installation, a Single Purpose Node requires to be configured as
Elastic-only or Voting-only: please refer to Section
:ref:`elastic-cluster` for guidelines.
