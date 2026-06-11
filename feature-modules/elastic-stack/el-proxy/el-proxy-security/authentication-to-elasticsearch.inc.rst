Authentication to Elasticsearch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When the Elasticsearch client is created, the authentication method to be used
to connect to Elasticsearch needs to be specified. The authentication method
defined in the configuration file is only used for the **serve** command.

The available authentication methods are:

-  **None**: the client connects to Elasticsearch without authentication. Example:

   .. code:: toml

      [elasticsearch.auth]
      type = "None"

-  **BasicAuth**: the client authenticates to Elasticsearch with username and password.
   When this method is used, the following information must be provided:

   -  **username**: name of the Elasticsearch user
   -  **password**: the password for the Elasticsearch user

   .. code:: toml

      [elasticsearch.auth]
      type = "BasicAuth"
      username = "myuser"
      password = "mypassword"

-  **PemCertificatePath**: the client connects to Elasticsearch using the PEM certificates read from the local
   file system. When this method is used, the following information must be provided:

   -  **certificate_path**: path to the public certificate accepted by Elasticsearch
   -  **private_key_path**: path to the corresponding private key

   Example:

   .. code:: toml

      [elasticsearch.auth]
      type = "PemCertificatePath"
      certificate_path = "/path/to/certs/ebp.crt.pem"
      private_key_path = "/path/to/certs/private/ebp.key.pem"
