.. _neteye-https-conf:

HTTPS Configuration
~~~~~~~~~~~~~~~~~~~

Beginning with version 4.2, NetEye has been configured to use HTTPS
throughout, using a self-signed certificate based on Apache's
*mod_ssl*. This certificate is generated automatically during the
NetEye install process.

However, it is recommended to create and install as soon as possible a
new trusted certificate consisting of the self-signed certificate
chained with a valid, external CA certificate.

The importance of using trusted certificates, is clear also from this
example use case: if you use the Director's Self-Service API to first
connect to an external Icinga2 agent, *the Kickstart initialization
script may fail* if it determines it **cannot trust the self-signed
certificate alone**. While this restriction can be bypassed as an
emergency measure, this is a highly insecure practice and is strongly
discouraged.

The following steps will help you configure your NetEye installation
to create and deploy a more secure certificate that can be trusted
externally and/or in your domain. If you would prefer to create a
certificate chain from within Windows, step by step instructions are
available :ref:`in the dedicated section
<windows-certificate-generation>`.

.. rubric:: Step 1: Obtain Your Signed Certificate from a Certificate Authority

The instructions below assume you already have a valid certificate from
an external Certificate Authority (CA). Then for each server/machine you
will need to:

1. Create a private key
2. Generate a certificate signing request (CSR)
3. Send or upload the private key and CSR to the CA
4. Retrieve the certificate signed by the CA

When NetEye is first installed, it configures the initial self-signed
certificates in the following directories:

+----------------------------+--------------------------------------------+
| File                       | Directory                                  |
+============================+============================================+
| *neteye\_cert.crt*         | */neteye/shared/httpd/conf/tls/certs/*     |
+----------------------------+--------------------------------------------+
| *neteye.key*               | */neteye/shared/httpd/conf/tls/private/*   |
+----------------------------+--------------------------------------------+
| *neteye\_chain.crt*        | */neteye/shared/httpd/conf/tls/certs/*     |
+----------------------------+--------------------------------------------+
| *neteye\_ca\_bundle.crt*   | */neteye/shared/httpd/conf/tls/certs/*     |
+----------------------------+--------------------------------------------+
| *neteye-ssl.conf*          | */etc/httpd/conf.d/*                       |
+----------------------------+--------------------------------------------+

.. note:: Certificate and key are located in shared directory. Therefore, in a
   cluster environment, they should be changed only on the node running
   the httpd resource.

Because the private key is so fundamentally important to your network's
security, you should strongly consider creating a new one. You can
create the private key and the CSR file in the appropriate directory
with a single command, after moving to the correct directory::

    # cd /neteye/shared/httpd/conf/tls/private/
    # openssl req -newkey rsa:4096 -nodes -keyout hostname.fqdn.key -out hostname.fqdn.csr
    Generating a 4096 bit RSA private key
    ..................................................................................................++
    ..................................................++
    writing new private key to 'hostname.fqdn.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [XX]:IT
    State or Province Name (full name) []:BZ
    Locality Name (eg, city) [Default City]:Bolzano
    Organization Name (eg, company) [Default Company Ltd]:Company
    Organizational Unit Name (eg, section) []:Monitoring
    Common Name (eg, your name or your server's hostname) []:hostname.fqdn
    Email Address []:mail@company.com

    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:

The *hostname.fqdn.key* file is your private key which should be kept
secure and not given to anyone. The *hostname.fqdn.csr* file is what you
should send to the Certificate Authority when requesting your SSL
certificate (you may need to paste its contents into the web form of the
CA).

.. note:: If you have a large number of systems to monitor, it makes sense to
   automate this process. For instance, you can keep multiple keys and
   CSRs manageable by using the host's FQDN as part of the filename for
   both the private key and the CSR. And rather than manually answer
   the CSR questions one by one, you can create an external
   configuration file (usually called openssl.cnf) that is invoked with
   the *-extfile* parameter.

Relevant links:

* `Description of OpenSSL/x509 and parameters
  :manpage:`openssl-x509` \
* `Example external configuration file
  <https://github.com/openssl/openssl/blob/master/apps/openssl.cnf>`_

.. rubric:: Step 2: How to Create the Trusted Certificate

The certificate that the CA returns to you (let's call it
*countersigned.crt*) will be the (self-signed) certificate you sent
them, countersigned with the CA's key. You can then use this new trusted
certificate in applications (e.g., browsers or the Icinga2 agent) that
in turn trust the CA you used.

To be used with the Icinga2 agent, this certificate should be in PEM
format. To check, you can look at the certificate file::

    # cat countersigned.crt
    -----BEGIN CERTIFICATE-----
    MIID3jCCAsagAwIBAgICPnowDQYJKoZIhvcNAQELBQAwgaMxCzAJBgNVBAYTAi0t
    ...
    -----END CERTIFICATE-----

If you do not see **BEGIN CERTIFICATE**, you will need to export the
certificate to PEM format (you can use other tools besides *openssl* as
long as they generate a certificate in PEM format)::

    # openssl x509 -in countersigned.crt -outform PEM -out countersigned.pem

.. rubric:: Step 3: Install the Certificates on the Web Server

You must then rename your certificates and key and move them in the
proper folder according to the settings in file
*/etc/httpd/conf.d/neteye-ssl.conf*::

    <VirtualHost _default_:443>
    SSLEngine on
    SSLCertificateFile /neteye/shared/httpd/conf/tls/certs/neteye_cert.crt
    SSLCertificateKeyFile /neteye/shared/httpd/conf/tls/private/neteye.key
    SSLCertificateChainFile /neteye/shared/httpd/conf/tls/certs/neteye_chain.crt
    SSLCACertificateFile /neteye/shared/httpd/conf/tls/certs/neteye_ca_bundle.crt
    </VirtualHost>

-  **SSLCertificateFile:** Your trusted certificate countersigned by the
   CA, from *countersigned.crt* (or *countersigned.pem* if you exported
   it in .pem format) to *neteye\_cert.crt*
-  **SSLCertificateKeyFile:** Your private key, renamed from
   *hostname.fqdn.key* to *neteye.key*
-  **SSLCertificateChainFile:** Certificate chain of the server
   certificate. If you don't have a chain you can copy the file
   *neteye\_cert.crt* naming it *neteye\_chain.crt*
-  **SSLCACertificateFile:** The CA's public certificate named
   *neteye\_ca\_bundle.crt*

.. note:: Do not change setting in file */etc/httpd/conf.d/neteye-ssl.conf*
   because they will be overwritten during update procedure possibly
   causing NetEye outages.

.. rubric:: Step 4: Restart Apache

Finally, restart the HTTPD service so it reloads the configuration files
above with the new trusted certificates. If you are on a single-node
instance use::

     # systemctl restart httpd.service

If you are on a cluster use::

     # pcs resource restart httpd
