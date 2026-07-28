.. _neteye-single-requirements:

Requirements for a Node
-----------------------

This section lists hardware and hypervisor requirements to install
|ne|. The system requirements are intended for a Single Node, a
Satellite Node, and *each* Cluster Node, and for both physical and
virtual installations.

System Requirements
~~~~~~~~~~~~~~~~~~~

:numref:`table-req` gives an overview of the basic system requirements
for a Node in both a testing and a production environment. Depending
on the services activated and the load on the system, requirements
might need to be raised. Indeed, running resource-intensive services
like the SIEM or ITOA would require to increase all the
requirements. Moreover, disk space may also become an issue when the
amount of logs produced by a |ne| installation or by its monitored
objects is very large.

You can always contact the official channels---sales,
consultants, or |support|_---for advice on how to tailor the system
according to your needs.

.. _table-req:

.. csv-table:: Minimum system requirements
   :header: "Requirement", "Demo or testing environment", "Production environment"

   "# of CPUs", "2 cores", "4 cores"
   "RAM", "8Gb", "16Gb"
   "Hard disk", "60Gb", "120Gb"

Starting from version *4.23*, |ne| is based on RHEL8, which requires a license
that is provided by |ne| sales or consultants. Since this license is necessary
to launch :command:`neteye install` during the installation procedure, make sure
you have it before starting the installation.

If your |ne| Node does not have direct Internet access and instead
needs to pass through a proxy to reach the Internet, you need
to configure the software running on |ne| to pass through this proxy,
as explained in the Section :ref:`nodes_behind_proxy`.

Moreover, the following domains must be reachable **from each
Node**, to allow for updates and license verification:

.. csv-table::
   :header: "Domain", "Port", "Intended Use"

   "repo.wuerth-phoenix.com", "443 TCP", "|witit| repository for |ne| update/upgrade"
   "api.neteye.cloud", "443 TCP", "|witit| API used during |ne| update/upgrade"
   "cdn.redhat.com", "443 TCP", "RedHat subscription/packages"
   "cdn-ubi.redhat.com", "443 TCP", "RedHat subscription/packages"
   "cert-api.access.redhat.com", "443 TCP", "RedHat subscription/packages"
   "cert.cloud.redhat.com", "443 TCP", "RedHat subscription/packages"
   "subscription.rhsm.redhat.com", "443 TCP", "RedHat subscription/packages"
   "mirrors.fedoraproject.org", "443 TCP", "Provides a set of additional packages for RHEL"
   "linux.dell.com", "443 TCP", "DELL packages (only for physical DELL machines)"
   "downloads.dell.com", "443 TCP", "DELL firmware updates"
   "downloads.dell-cidr.akadns.net", "443 TCP", "DELL firmware updates"
   "downloads-regions.dell-cidr.akadns.net", "443 TCP", "DELL firmware updates"
   "downloads.dell.com-v2-dd.edgekey.net", "443 TCP", "DELL firmware updates"
   "e12616.dscd.akamaiedge.net", "443 TCP", "DELL firmware updates"
   "2.rhel.pool.ntp.org", "123 UDP", "NTP server for time synchronization. Alternatively, an internal NTP server can be `configured <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/configuring-time-synchronization_configuring-basic-system-settings#setting-up-chrony-for-a-system-in-an-isolated-network_using-chrony>`__"

The following domains may prove to be useful and simplify working with |ne|:

.. csv-table::
   :header: "Domain", "Port", "Intended Use"

   "bitbucket.org", "443 TCP", "Download customized script and plugin (often used by |ne| consultants)"
   "grafana.com", "443 TCP", "Download Grafana plugins, panels and datasources"
   "yum.centreon.com", "443 TCP", "Download Centreon plugins for monitoring"


.. _neteye-virtual-req:

Supported Virtualization Environments
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| installation is supported in the following virtualization
environments. For each of them, some options are listed that need to be
configured during installation.

* **VMware**. Select as *ESXi 6.7 and Later* as **Compatibility**,
  *then VMware Paravirtual* as **SCSI controller**, and finally either
  *SATA* or *SCSI*.

* **KVM**.  In **Boot Options** check that *Disk1* and *CDRom* are
  both selected, then change the disk bus to *SATA* (**VirIO Disk 1**
  under **Advanced options** in the next configuration step).

* **HyperV**. No particular option is required.

.. _ldap-access-reqs:

LDAP Access Requirements
~~~~~~~~~~~~~~~~~~~~~~~~

|ne| 4 is using LDAP protocol for the purpose of binding users within
Active Directory, OpenLDAP, to a centralized account. Hence, |ne| adheres
to the `LDAP Standards Track <https://datatracker.ietf.org/doc/html/rfc4511>`__.

In order to log in to |ne| 4 with a centralized account, create an
LDAP/AD user with read permissions on the following objects:

* Account name
* Password
* Email address

You will also need to open several :ref:`TCP ports <table-cluster-tcp-communication-req>`
from |ne| 4 to the LDAP system directory.


.. _notification-req:

Notification Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~

Notifications can be sent via SMTP or SMS. Therefore, the following requirements should be satisfied.

* To send notifications via SMTP you need an SMTP Relay Server, which should be reachable by |ne| Nodes as described
  :ref:`here <table-cluster-tcp-communication-req>`

* In order to send SMS messages, unset the :ref:`PIN on your SIM card
  <sms-modem-setup>`

  * To handle SMS we provide two types of modem:

    * SMS Gateway connected over Ethernet
    * :ref:`SMS Gateway connected via serial bus <sms-gateway-moxa>`
      (contact your |ne| 4's consultant for further information)

Kubernetes Requirements
~~~~~~~~~~~~~~~~~~~~~~~
Since |ne| 4.49, RKE2 is used as the Kubernetes distribution. RKE2 requires three CIDRs to be defined during the installation process in the :file:`/etc/neteye-environment.yaml` file.
The following three CIDRs are required:

- `pod_cidr`: the CIDR from which the pods will be assigned their IP addresses. The default value is `10.42.0.0/16`. Regardless of the chosen value, the CIDR must be a /16 network.
- `svc_cidr`: the CIDR from which the services will be assigned their IP addresses. The default value is `10.43.0.0/16`. Regardless of the chosen value, the CIDR must be a /16 network.
- `service_loadbalancer_cidr`: the CIDR from which the service load balancers will be assigned their IP addresses. The default value is `10.44.0.0/24`. Regardless of the chosen value, the CIDR must contain at least 256 addresses (`/24` or lower).

Changing the CIDRs after the initial installation is currently **not** supported and may lead to inconsistent behaviours of the deployed components.
