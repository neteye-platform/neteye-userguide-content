NGINX is installed and enabled by default on Satellites and is responsible to expose local services,
like Tornado Webhook collector, and to perform TLS termination.
NGINX can be customised to some extent, to be employed in other scenarios like those described below.

Change NGINX Certificates
`````````````````````````

By default NGINX is configured with self-signed certificates generated at Satellite side.
To use your own certificates you **must not** change the NGINX configuration, but you can overwrite
the existing self-signed certificates in the following locations:

* **Certificate:** it is mandatory and located in :file:`/neteye/local/nginx/conf/tls/certs/neteye_cert.crt`
* **Key:** it is mandatory and located in :file:`/neteye/local/nginx/conf/tls/private/neteye.key`
* **CA or CA bundle:** it is mandatory and located in :file:`/neteye/local/nginx/conf/tls/certs/neteye_ca_bundle.crt`

Setup a Reverse Proxy for Https Resource
````````````````````````````````````````

In this scenario we assume that you want to forward all HTTPS requests for `neteyeshare`
to the master.


If you are familiar with `Httpd`, the corresponding configuration would look like this:

.. code:: apache

  ProxyPass /neteyeshare https://neteye4master.example.it/neteyeshare
  ProxyPassReverse /neteyeshare https://neteye4master.example.it/neteyeshare


To configure NGINX as a reverse proxy you should create file
:file:`/neteye/local/nginx/conf/conf.d/http/locations/neteyeshare.conf`
with the following content:

.. code:: nginx

    location /neteyeshare/ {
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass https://neteye4master.example.it/neteyeshare;
    }

You need to restart NGINX to apply changes.

Setup a Server in NGINX
```````````````````````

By default NGINX on Satellites listen only to port `443`. It is possible to start a new
server to listen on a different port, for example to set it up as reverse proxy.

In this case you need to create a new file
:file:`/neteye/local/nginx/conf/conf.d/http/my_custom_server.conf`
with the following content:

.. code:: nginx

   server {
     listen 80;
     server_name my_custom_server;
     location /api/v1/ {
         proxy_set_header X-Forwarded-Host $host:$server_port;
         proxy_set_header X-Forwarded-Server $host;
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_pass http://127.0.0.1:8080/api/v1;
     }
   }

You need to restart NGINX to apply changes.

Change SSL settings
```````````````````

Unlike the previous scenarios, this settings must be configured and applied on the Master;
then you need to follow the instructions in sections :ref:`neteye-satellite-config-create`,
:ref:`neteye-satellite-config-send` and :ref:`neteye-satellite-setup`, to deploy configuration
on Satellite.

To change NGINX SSL settings you can change optional parameters
``proxy.ssl_protocol`` and ``proxy.ssl_cipher_suite`` described in
:ref:`NetEye Satellite Configuration <neteye-satellite-config-params>`.

Suppose the Satellite configuration :file:`/etc/neteye-satellite.d/tenant_A/acmesatellite.conf`
is the following:

.. code:: json

   {
     "fqdn": "acmesatellite.example.com",
     "name": "acmesatellite",
     "ssh_port": "22",
     "ssh_enabled": true
   }

Let's suppose you want to setup NGINX to support ``TLSv1.2`` only. You have just
to set your satellite configuration file
:file:`/etc/neteye-satellite.d/tenant_A/acmesatellite.conf`
file as follows:

.. code:: json

   {
     "fqdn": "acmesatellite.example.com",
     "name": "acmesatellite",
     "ssh_port": "22",
     "ssh_enabled": true,
     "proxy": {
       "ssl_protocol": "TLSv1.2"
     }
   }
