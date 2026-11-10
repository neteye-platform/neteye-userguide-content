.. _kubernetes-ca-trust:

Trusted CAs for Kubernetes Workloads
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The |ne| components that run on Kubernetes, such as Keycloak and the OpenTelemetry Collector, do not use the
Certificate Authorities (CAs) trusted by the node they run on, for example those added with
:command:`update-ca-trust`. They trust only:

- the well-known public CAs shipped with their container image;
- the |ne| root CA;
- the CAs you add to the :file:`/neteye/local/rke2/conf/ca-trust` directory, described below.

If a workload connects to a service whose certificate is issued by a private CA, you must add that CA to the
directory. A typical example is Keycloak connecting to an LDAPS server (see :ref:`ldap-configuration`) or to an
external identity provider. Without the CA, the TLS handshake fails.

.. note:: Every certificate in the directory is trusted by all the workloads above. Add only the CAs that are really
   needed, and never a CA you do not trust.

The :file:`ca-trust` directory
``````````````````````````````

The directory is created by |ne| and contains:

- :file:`00-neteye-root-ca.crt`, the |ne| root CA. It is managed by |ne| and overwritten at every :command:`neteye
  install`, :command:`neteye update` and :command:`neteye upgrade`: do not edit or remove it.
- Your own CAs. Add them as files with the :file:`.crt` or :file:`.pem` extension, in PEM format. A file may contain
  more than one certificate.

Subdirectories, hidden files and files with any other extension are ignored. Files that you add are never modified or
removed by |ne|.

Since anyone able to change this directory or its files could make the workloads trust a CA of their choice, the
directory and every :file:`.crt` and :file:`.pem` file in it (a symbolic link is followed) must be owned by ``root`` and
must not be writable by their group or by others. Otherwise :command:`neteye install`, :command:`neteye update` and
:command:`neteye upgrade` stop with an error naming the path.

To add a CA, for example the one of an LDAPS server, copy its certificate into the directory:

.. code-block:: bash

   neteye# cp ldap-ca.crt /neteye/local/rke2/conf/ca-trust/

.. note:: On a cluster, the directory must contain the same files on **all** the nodes, so copy the files to every
   node. :command:`neteye install`, :command:`neteye update` and :command:`neteye upgrade` stop with an error that
   lists the checksum of each node if the directory differs.

Apply the changes
`````````````````

The content of the directory is merged into a single bundle, stored in the ``neteye-ca-bundle`` ConfigMap of the
``neteye-tenant-shared`` Kubernetes namespace. The |ne| Operator mounts it in the workloads. This happens automatically
during :command:`neteye install`, :command:`neteye update` and :command:`neteye upgrade`. To apply a change at any
other time, run:

.. code-block:: bash

   neteye# neteye install --restrict-services-to neteye-operator

When the bundle changes, Keycloak and the OpenTelemetry Collector are restarted to load it. Nothing is restarted if
the content did not change.

While merging, |ne| re-encodes every certificate, removes duplicates and comments, and refuses to continue, naming the
guilty file, if:

- a file does not contain any PEM certificate, or contains a malformed one;
- a file contains a private key: never put private keys in this directory;
- the resulting bundle is larger than 900 KB.

To check which CAs were merged, count the certificates in the bundle (the |ne| root CA and one for each certificate you
added, minus duplicates):

.. code-block:: bash

   neteye# kubectl get configmap neteye-ca-bundle -n neteye-tenant-shared \
       -o jsonpath='{.data.ca-bundle\.pem}' | grep -c 'BEGIN CERTIFICATE'
