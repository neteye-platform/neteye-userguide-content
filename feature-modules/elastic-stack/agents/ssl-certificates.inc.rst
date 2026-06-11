Use of SSL certificates
~~~~~~~~~~~~~~~~~~~~~~~

Server certificates of Logstash allowing communication with Beats must
be stored in the ``/neteye/shared/logstash/conf/certs/`` directory, with
names **logstash-server.crt.pem** and **private/logstash-server.key**.
Additionally, also the **root-ca.crt** certificate must be available in
the same directory.

The structure mentioned above for the certificates must be organised as::

  certs/
     ├── logstash-server.crt.pem
     ├── root-ca.crt
     └── private/
            └── logstash-server.key

The certificates are stored under the ``logstash`` configuration
directory, because it is indeed Logstash that listens for incoming Beat
data flows.

As a consequence, all Beat clients must use a client certificate to
send output data to Logstash. Please refer to `the Elastic official
documentation <https://www.elastic.co/guide/index.html>`__, for
example the Filebeat SSL configuration is available `here
<https://www.elastic.co/guide/en/beats/filebeat/7.17/configuration-ssl.html>`__.

An example of Filebeat to Logstash SSL communication configuration is
the following::

    #--------- Logstash output ------------------------------------
        output.logstash:
          # The Logstash hosts
          hosts: ["yourNetEyeDomain.example:5044"]

          # List of root certificates for HTTPS server verifications
          ssl.certificate_authorities: ["/root/beat/root-ca.crt"]

          # Certificate for SSL client authentication
          ssl.certificate: "/root/beat/logstash-client.crt.pem"

          # Client Certificate Key
          ssl.key: "/root/beat/private/logstash-client.key.pem"

Self-signed certificates
````````````````````````

.. note:: For production systems, you should upload **your own certificates** on NetEye.
   Moreover, you should use your own certificates also for all Beat clients.
   Self-signed certificates must **never** be used on production systems,
   but **only** for testing and demo purposes.

**Self-signed certificates** (logstash-server.crt.pem and
private/logstash-server.key) and the Root CA (root-ca.crt) are shipped
with NetEye for Logstash. **Self-signed certificates** for Beat clients
can be generated from the CLI as follows:

you can run the script ``usr/share/neteye/scripts/security/generate_client_certs.sh`` using three suitable parameters:
 * The client name
 * The common name (CN) and information for the other certificate's field
 * The output directory

An example of command line is the following:

.. code:: bash

    /bin/bash /usr/share/neteye/scripts/security/generate_client_certs.sh \
        logstash-client \
        "/CN=logstash-client/OU=client/O=client/L=Bolzano/ST=Bolzano/C=IT" \
        "/root/beat/"


Inputs configuration
````````````````````

To set customer-specific filebeat inputs you can add a file with :file:`.yml` extension in the directory
:file:`/neteye/shared/filebeat/conf/inputs.d/`.
Configuration will be read and applied from :file:`.yml` files only: any file with different extension will be ignored.
To maintain a custom configuration saved but **disabled**, you should rename the file with a different extension,
for example :file:`mqtt.yml` can be disabled by renaming it to :file:`mqtt.yml.disable`.

A sample configuration can be found in file :file:`/neteye/shared/filebeat/conf/inputs.d/mqtt.yml.sample`.
