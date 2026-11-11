.. _kubernetes-ca-trust:

Trusted CAs for Kubernetes Workloads
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The |ne| components that run on Kubernetes, such as Keycloak and the OpenTelemetry Collector, do not use the
Certificate Authorities (CAs) trusted by the node they run on, for example those added with
:command:`update-ca-trust`. They trust only:

- The well-known public CAs shipped with their container image
- The |ne| root CA
- The CAs you add to the :file:`/neteye/local/rke2/conf/ca-trust/` directory, described below

If a workload connects to a service whose certificate is issued by a private CA, you must add that CA to the
directory. A typical example is when Keycloak connects to an LDAPS server (see :ref:`ldap-configuration`) or to an
external identity provider. Without a valid CA, TLS handshakes will fail.

The workloads also connect to the |ne| frontend domain. If its certificate is not issued by the |ne| root CA, for
example because you configured a custom certificate for the web interface, the CA that issued it must be in the
directory as well.

.. note:: Every certificate in the directory is trusted by all the workloads above. Add only the CAs that are really
   needed, and never a CA you do not trust.

The :file:`ca-trust` Directory
``````````````````````````````

The directory is created by |ne| and contains:

- :file:`00-neteye-root-ca.crt`, the |ne| root CA. It is managed by |ne| and overwritten at every :command:`neteye
  install`, :command:`neteye update` and :command:`neteye upgrade`: do not edit or remove it.
- Your own CAs. Add them as files with the :file:`.crt` or :file:`.pem` extension, in PEM format. A file may contain
  more than one certificate.

When |ne| adds :file:`00-neteye-root-ca.crt` for the first time, it also pre-populates the directory with the CAs
trusted by the node, so that the workloads keep trusting the CAs you already added to the node. It copies the
:file:`.crt` and :file:`.pem` files of :file:`/etc/pki/ca-trust/source/anchors/` and
:file:`/usr/share/pki/ca-trust-source/anchors/`, with the following exceptions:

- :file:`root-ca.crt`, the |ne| root CA, already present as :file:`00-neteye-root-ca.crt`
- :file:`neteye-icinga2-master-ca.crt`, the Icinga 2 master CA, which the workloads do not need
- Files with the same content as one already copied. If both directories contain a file with the same name or the same
  content, the one in :file:`/etc/pki/ca-trust/source/anchors/` is copied.

Pre-population happens only once: the CAs you add to the node afterwards are not copied, and the files you remove from
the directory are not copied again. Review the copied files and **remove** those that the workloads do not need.

The trust store of the node also accepts certificates in other formats, such as DER, which the workloads cannot use.
Such files are copied as they are, and the upgrade prerequisite checks report them. Convert each of them to PEM, for
example:

.. code-block:: bash

   neteye# cd /neteye/local/rke2/conf/ca-trust/
   neteye# openssl x509 -inform DER -in my-ca.crt -out my-ca.pem && rm my-ca.crt

or **remove** it if the workloads do not need that CA.

Subdirectories, hidden files and files with any other extension are ignored. Files that you add are never modified or
removed by |ne|.

Since anyone able to change this directory or its files could make the workloads trust a CA of their choice, the
directory and every :file:`.crt` and :file:`.pem` file in it (a symbolic link is followed) must be owned by ``root`` and
must not be writable by their group or by others. Otherwise :command:`neteye install`, :command:`neteye update` and
:command:`neteye upgrade` stop with an error naming the path.

To add a CA, for example the one for an LDAPS server, copy its certificate into the directory:

.. code-block:: bash

   neteye# cp ldap-ca.crt /neteye/local/rke2/conf/ca-trust/

.. note:: On a cluster, the directory must contain the same files on **all** the nodes, so copy the files to every
   node. :command:`neteye install`, :command:`neteye update` and :command:`neteye upgrade` stop with an error that
   lists the checksum of each node if the directory differs.

Confirm the Directory Before Upgrading to |ne| 4.51
```````````````````````````````````````````````````

The directory is created, and pre-populated, by the latest updates of |ne| 4.50. Before upgrading to |ne| 4.51:

#. On **every** node, review the content of :file:`/neteye/local/rke2/conf/ca-trust/`: **remove** the CAs that the
   workloads do not need and add the missing ones, for example the CA of an LDAPS server that is not trusted by the
   node, or the CA that issued the certificate of the |ne| frontend domain if it is not the |ne| root CA. Make sure
   that all the nodes contain the same files.
#. Confirm that the directory is ready: in Icinga Web 2, open the configuration of the *neteye* module, check **I am
   ready to upgrade** and save. Alternatively, run:

   .. code-block:: bash

      neteye# icingacli neteye catrustready enable

The upgrade prerequisite checks fail until the confirmation is given. They also fail, on each node, if the directory
or its files can be changed by a user other than ``root``, and if a :file:`.crt` or :file:`.pem` file is empty, does not
contain any certificate in PEM format, contains a malformed certificate or contains a private key. For each problem,
they name the file and tell how to fix it. The checks are skipped on |ne| Satellites, as they do not run Kubernetes
workloads.

Apply the Changes
`````````````````

The content of the directory is merged into a single bundle, stored in the ``neteye-ca-bundle`` ConfigMap of the
``neteye-tenant-shared`` Kubernetes namespace. The |ne| Operator mounts it in the workloads. This happens automatically
during :command:`neteye install`, :command:`neteye update` and :command:`neteye upgrade`. To apply a change at any
other time, run:

.. code-block:: bash

   neteye# neteye install --restrict-services-to neteye-operator

When the bundle changes, Keycloak and the OpenTelemetry Collector are restarted to load it. Nothing is restarted if
the content does not change.

While merging the CAs, |ne| re-encodes every certificate, removes duplicates and comments, and refuses to continue,
naming the guilty file, if:

- A file does not contain any PEM certificate, or contains a malformed one
- A file contains a private key (never put private keys in this directory)
- The resulting bundle is larger than 900 KB

To check which CAs were merged, count the certificates in the bundle (the |ne| root CA and one for each certificate you
added, minus duplicates):

.. code-block:: bash

   neteye# kubectl get configmap neteye-ca-bundle -n neteye-tenant-shared \
       -o jsonpath='{.data.ca-bundle\.pem}' | grep -c 'BEGIN CERTIFICATE'
