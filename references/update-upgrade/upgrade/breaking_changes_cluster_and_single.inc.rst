|ne| Kubernetes Operator
~~~~~~~~~~~~~~~~~~~~~~~~

This release will introduce a brand new |ne| Operator which is the component responsible for managing the |ne|
deployment in Kubernetes. All configuration and management is made by the standard |ne| CLI procedures such as
:command:`neteye install` and :command:`neteye upgrade`, but you can configure and override the default configuration by
editing the :file:`/etc/neteye-environment.yaml` file. |ne| CLI procedures will automatically source this configuration
file and apply desired changes to the Operator. This upgrade does not require users to update the configuration
beforehand since default values will be applied. For the full list of configurable parameters, please refer to the
:file:`/usr/share/neteye/setup/neteye-environment.yaml.tpl` file. To apply the configuration changes during normal
operation, you can use the :command:`neteye install --restrict-services-to neteye-operator` command. This command will
apply the configuration changes to the Operator without affecting the other services.

|ne| CA Trust Bundle for Kubernetes Workloads
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To ensure that all Kubernetes workloads trust the various Certificate Authorities (CAs) used by external services, for
example, when connecting to LDAP servers with Keycloak, users are now required to configure the |ne| CA Trust.
To do so, a new configuration directory has been introduced at :file:`/neteye/local/rke2/conf/ca-trust`.

This is a **breaking change**: the workloads running on Kubernetes no longer trust the CAs installed on the node (for
example with :command:`update-ca-trust`), but only the public CAs shipped with their image, the |ne| root CA and the CAs
in this directory. If, before the upgrade, Keycloak relied on a CA added to the node to reach an LDAPS server or an
external identity provider, that CA must be added to the directory, otherwise the connection fails after the upgrade.

The directory is created, with the |ne| root CA in it, by the latest updates of |ne| 4.50. Before upgrading:

#. Identify the private CAs of the services that Keycloak and the other workloads (OtelCollector) connect to.
#. Copy their certificates, in PEM format with the :file:`.crt` or :file:`.pem` extension, into
   :file:`/neteye/local/rke2/conf/ca-trust` of **every** node of the installation. The directory and the files must be
   owned by ``root`` and not writable by their group or by others, otherwise the upgrade prerequisite checks fail on that
   node and tell how to fix it.
#. Confirm it in Icinga Web 2: in the configuration of the *neteye* module, check **I am ready to upgrade** and save.

The upgrade prerequisite checks fail until the confirmation is given. The check is skipped on |ne| Satellites, as they
do not run Kubernetes workloads. The directory is merged into the bundle used by the workloads during the upgrade.

The directory stays in use after the upgrade, when you need to trust new CAs. For its content, the format of the
files and the commands to apply a change, see :ref:`kubernetes-ca-trust`.
