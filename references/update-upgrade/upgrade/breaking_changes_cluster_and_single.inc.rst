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
To do so, a new configuration directory has been introduced at :file:`/neteye/local/rke2/conf/ca-trust/`.

This is a **breaking change**: the workloads running on Kubernetes will no longer trust the CAs installed on the node
(for example with :command:`update-ca-trust`), but only the public CAs shipped with their image, the |ne| root CA and
the CAs in this directory. If, before the upgrade, Keycloak relied on a CA added to the node to reach an LDAPS server or
an external identity provider, that CA must be added to the directory, otherwise the connection will fail after the
upgrade.

The directory is created by the latest updates of |ne| 4.50 and pre-populated with the |ne| root CA and the CAs trusted
by the node, that is, the :file:`.crt` and :file:`.pem` files in :file:`/etc/pki/ca-trust/source/anchors/` and
:file:`/usr/share/pki/ca-trust-source/anchors/`, except for the |ne| root CA and the Icinga 2 master CA, which are
already handled by |ne|. Before upgrading:

#. Identify the private CAs of the services that Keycloak and the other workloads (OpenTelemetry Collector) connect to,
   including the CA that issued the certificate of the |ne| frontend domain, if it is not the |ne| root CA.
#. Review :file:`/neteye/local/rke2/conf/ca-trust/` on **every** node of the installation: **remove** the pre-populated
   CAs that the workloads do not need and add the missing ones, in PEM format with the :file:`.crt` or :file:`.pem`
   extension. All the nodes must contain the same files. The directory and the files must be owned by ``root`` and not
   writable by their group or by others, and every :file:`.crt` and :file:`.pem` file must contain only valid
   certificates in PEM format (the files copied from the node may also be in other formats, such as DER). Otherwise the
   upgrade prerequisite checks will fail on that node and explain how to fix each file, or advise removing it if the
   workloads do not need that CA.
#. Confirm it in Icinga Web 2: in the configuration of the *neteye* module, check **I am ready to upgrade** and save,
   or run :command:`icingacli neteye catrustready enable`.

The upgrade prerequisite checks will fail until the confirmation is given. The check is skipped on |ne| Satellites, as
they do not run Kubernetes workloads. The directory will be merged into the bundle used by the workloads during the
upgrade.

The directory will remain after the upgrade in case you need to trust new CAs. For the content, the pre-population, the
format of the files and the commands to apply a change, see :ref:`kubernetes-ca-trust`.
