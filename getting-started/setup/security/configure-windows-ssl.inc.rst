.. _windows-certificate-generation:

Trusted Certificate Generation with Windows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The instructions below will help you create a trusted certificate
chain in Windows and configure HTTPS in NetEye 4 with the trusted
certificate.  There are also instructions for how to do this from
:ref:`within the NetEye server <neteye-https-conf>`.

Requirements
````````````

The following requirements should be met before proceeding with
configuration of the certificate:

-  A Windows Certification Authority should already be up and running,
   with a suitable Certificate Template.
-  The Certificate Template should meet recent encryption standards, for
   example:

   -  RSA, SHA256, 4096bit
   -  The private key should be marked as exportable

-  You have a Windows domain-joined Server/PC which is allowed to
   request certificates.
-  You have a Linux machine running NetEye where you can install the CA
   Chain Certificate. This is necessary for the server certificate to be
   trusted by the Apache web server.

Procedure
`````````

.. rubric:: **Step 1:** Request a new certificate from a Windows
   domain-joined Server/PC:

-  Open the Microsoft Management Console (:menuselection:`Start --> mmc.exe`)
-  Within MMC, go to :menuselection:`File --> Add/Remove Snap-in`
-  In the popup dialog, navigate to :menuselection:`Certificates --> Add --> Computer
   Account --> Next --> Local computer --> Finish`, and then **OK** to
   close the dialog.
-  Expand :menuselection:`Certificates --> Personal`, then right click on
   **Certificates**
-  Select **All Tasks**, then **Request new certificate** (you may
   need to skip a “Before You Begin” screen first) and **Next** when
   “Active Directory Enrollment Policy” is selected as shown here:
   |Enrollment - Select Policy|
-  Select a Certificate Template and click on the link “More
   information is required to enroll for this certificate…”
   |Enrollment - Request Certificates|
-  Fill-in the information for each tab in the **Certificate
   Properties** dialog as shown (the fields shown are mandatory; you
   can optionally add values like Country, Department, Organization,
   etc.): |Enrollment - Subject Properties| |Enrollment - General
   Properties| |Enrollment - Extensions Properties 1| |Enrollment -
   Extensions Properties 2| |Enrollment - Private key Properties|
-  Click **OK** and then **Enroll** and **Finish**.

.. rubric:: **Step 2:** Export the certificate with its private key in
   PFX format:

-  Right click on the certificate you just created in the center
   panel, then click on :menuselection:`All Tasks --> Export`.
-  Select **Yes, export the private key**, click **Next**, and select
   **PKCS #12**: |Enrollment - Export Wizard File Format|
-  Provide a password to protect the private key that goes with the
   certificate (a strong password is advised): |Enrollment - Export
   Wizard Security|
-  Then designate the path where the certificate should be stored:
   |Enrollment - Export Wizard File Export|
-  Now click **Next**, and then **Finish**. You should see the
   message “The export was successful.”

.. rubric:: **Step 3:** Export your CA Certificate(s) in Base64
   format.

.. note:: If the Certification Authority Infrastructure consists of
   multiple CAs (for example, Root CA > Subordinate Intermediate CA),
   you must export all of them and then combine them into a single
   Certificate.

-  Double click on your new certificate in the center panel. In the
   popup dialog, click on the **Certification Path** tab, which
   should display a Certificate Chain such as the one shown here:
   |Enrollment - Certificate Chain|
-  Select for instance the Intermediate Certificate, and then click
   on the **View Certificate** button. Then click on the **Details >
   Copy to File**: |Enrollment - Certification Path|
-  Instead of the “DER encoded binary” option, select “Base-64
   encoded”: |Enrollment - Certificate Export Format|
-  In the next dialog, choose a path and filename to save the .CER
   file, then click **Finish**.
-  Repeat the procedure above for the Root CA instead of the
   intermediate certificate.
-  To create the certificate chain, open all of your saved CA
   certificates in a Text Editor and combine them into a single file,
   both respecting the proper order (Root/Parent before
   Subordinate/Child) and paying attention to not leave any blank
   lines between certificates as shown here: |Enrollment -
   Certificate Base64|\

.. rubric:: **Step 4:** Copy the CA Chain certificate to the Linux
   server (NetEye 4) and adjust the Apache configuration:

-  Make a copy of the CA Chain certificate, rename and move both of
   them to the proper folder according to the settings in
   :file:`/etc/httpd/conf.d/neteye-ssl.conf` file:

   .. container:: codeblock:

      .. code::

         SSLCertificateChainFile /neteye/shared/httpd/conf/tls/certs/neteye_chain.crt
         SSLCACertificateFile /neteye/shared/httpd/conf/tls/certs/neteye_ca_bundle.crt

.. rubric:: **Step 5:** Copy your PFX Server Certificate to the Linux
   server (NetEye 4), convert it, and adjust the Apache configuration:

-  Put the PFX certificate in a temporary directory, for example
   */tmp*.
-  Extract the public part of the certificate. You will be asked for
   the Key Password, which is the one you entered when you exported
   your PFX from the Windows Machine::

     # openssl pkcs12 -in {yourfile.pfx} -nokeys -out {certificate.crt}

-  Extract the encrypted private key part of the certificate. You
   will be asked for the Key Password, which is the one you entered
   when you exported your PFX file from the Windows Machine. You will
   also be asked to enter a new Password for the newly generated
   private key (you can use the same password)::

     #  openssl pkcs12 -in {yourfile.pfx} -nocerts -out {keyfile-encrypted.key}

-  Now decrypt your private key::

     # openssl rsa -in {keyfile-encrypted.key} -out {keyfile-decrypted.key}

-  Rename your certificate and key and move them in the proper folder according
   to the settings in :file:`/etc/httpd/conf.d/neteye-ssl.conf` file:

   .. container:: codeblock:

      .. code::

         SSLCertificateFile /neteye/shared/httpd/conf/tls/certs/neteye_cert.crt
         SSLCertificateKeyFile /neteye/shared/httpd/conf/tls/private/neteye.key

.. note:: Both the certificate and key must be owned by root and only root must
   have full read and write access to the files. Also, certificate and key are located
   in shared directory. Therefore, in a cluster environment, they should be changed
   only on the node running the httpd resource.

-  Finally, restart Apache.

.. |Enrollment - Select Policy|  image:: /getting-started/setup/img/win-cert-01.png
.. |Enrollment - Request Certificates|  image:: /getting-started/setup/img/win-cert-02.png
.. |Enrollment - Subject Properties|  image:: /getting-started/setup/img/win-cert-03.png
.. |Enrollment - General Properties|  image:: /getting-started/setup/img/win-cert-04.png
.. |Enrollment - Extensions Properties 1|  image:: /getting-started/setup/img/win-cert-05.png
.. |Enrollment - Extensions Properties 2|  image:: /getting-started/setup/img/win-cert-06.png
.. |Enrollment - Private key Properties|  image:: /getting-started/setup/img/win-cert-07.png
.. |Enrollment - Export Wizard File Format|  image:: /getting-started/setup/img/win-cert-08.png
.. |Enrollment - Export Wizard Security|  image:: /getting-started/setup/img/win-cert-09.png
.. |Enrollment - Export Wizard File Export|  image:: /getting-started/setup/img/win-cert-10.png
.. |Enrollment - Certificate Chain|  image:: /getting-started/setup/img/win-cert-11.png
.. |Enrollment - Certification Path|  image:: /getting-started/setup/img/win-cert-12.png
.. |Enrollment - Certificate Export Format|  image:: /getting-started/setup/img/win-cert-13.png
.. |Enrollment - Certificate Base64|  image:: /getting-started/setup/img/win-cert-14.png
