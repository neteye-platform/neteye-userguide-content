

The |ne| install process creates a self-signed root Certificate
Authority in :file:`/root/security/`. This CA is synchronized
throughout the |ne| Cluster.

The common CA is trusted automatically during installation with
:command:`neteye install`, leveraging the
:command:`update-ca-trust` script to `update certificate authorities
<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/securing_networks/using-shared-system-certificates_securing-networks>`__
provided by the system. Once the CA is in place, each module on each
cluster node can request its certificate which is then signed by the
common CA.

By default, the |ne| CA is stored in :file:`/root/security/` and trust
settings are set in :file:`/usr/share/pki/ca-trust-source/`. That directory
contains CA certificates and trust settings in the PEM file format, and
these are interpreted as a default priority. This setting allows the
administrator to override the CA certificate list. Of course, for
correct trust behavior, the |ne| CA should not be overwritten.


Certificates Storage
~~~~~~~~~~~~~~~~~~~~

Each component that uses certificates stores them in its :file:`conf`
folder, under the directory :file:`certs`.

* For example, Elasticsearch stores the certificates in the path
  :file:`/neteye/local/elasticsearch/conf/certs/`

The :file:`certs` folder contains the public certificates, while the private
keys are stored inside :file:`certs/private/`.

* For example, the public certificate of the Elasticsearch admin is
  stored in
  :file:`/neteye/local/elasticsearch/conf/certs/admin.crt.pem`, while
  its private key is stored in
  :file:`/neteye/local/elasticsearch/conf/certs/private/admin.key.pem`

Some components export the certificates in *PKCS 12* bundles (*.pfx*
files) inside the folder :file:`certs/private/`. These bundles contain the
private key together with its corresponding certificate. If not
specified otherwise, the password to decrypt the *pfx* files is
blank (i.e. empty password).

* For example, Tornado exports its certificate and private key to
  *PKCS 12* format inside the file
  :file:`/neteye/shared/tornado/conf/certs/private/tornado.pfx`. This
  can be decrypted by using an empty password::

    openssl pkcs12 -in /neteye/shared/tornado/conf/certs/private/tornado.pfx -nodes -password pass:
