Secure Communication
~~~~~~~~~~~~~~~~~~~~

When installed on |ne|, the El Proxy automatically starts in secure mode
using TLS. Additionally, authentication with Elasticsearch is protected
by certificates.

TLS Configuration
`````````````````
Advanced users should be able to check the location of the configuration files
or modify the setup.

The El Proxy server can start in HTTP or HTTPS mode; this is
configured in the config ``web_server.tls`` section.

The available modes are:

-  **None**: The El Proxy server starts with TLS disabled. Example:

   .. code:: toml

      [web_server.tls]
      type = "None"


-  **PemCertificatePath**: The El Proxy server starts with TLS enabled using the PEM certificates read from the local
   file system. When this method is used, the following information must be provided:

   -  **certificate_path**: path to the server public certificate
   -  **private_key_path**: path to the server private key


   Example:

   .. code:: toml

      [web_server.tls]
      type = "PemCertificatePath"
      certificate_path = "/path/to/certs/ebp_server.crt.pem"
      private_key_path = "/path/to/certs/private/ebp_server.key.pem"
