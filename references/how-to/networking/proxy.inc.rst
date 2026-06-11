.. _howto-networking-proxy:

How To Setup a reverse proxy in front of NetEye
-----------------------------------------------

If your infrastructure requires a `reverse proxy` set up in front of NetEye, there are some requirements to follow for a correct implementation.
The main requirement is that the `Host` headers **MUST** be **preserved** when forwarding requests to a backend server.

This page shows you some examples of Nginx or Apache configurations.

**Nginx**

.. code::

    [...]
    proxy_pass http://backend_server_address;
    proxy_set_header Host $host;
    [...]


**Apache**

.. code::

    [...]
    ProxyPreserveHost On
    ProxyPass / http://backend_server_address/
    [...]


IP Setting
----------

For `login auditing` purposes, |ne| should know the `IP` address of the client. By default it takes the `REMOTE_ADDR` variable. In reverse proxy setup this is not
the real `IP`, under :menuselection:`Configuration / Modules / NetEye / Configuration` there is a config option called `Proxy IP Mode` that can be set to `X-Forwarded-For` or `Client-IP`.

When set to one of these values, the `Proxy` **MUST** overwrite (or add) the chosen header with the real client IP address.

**Nginx**

.. code::

    [...]
    proxy_set_header X-Forwarded-For $remote_addr;  // or proxy_set_header Client-IP $remote_addr;
    [...]

**Apache**

.. code::

    [...]
    RequestHeader set X-Forwarded-For "%{REMOTE_ADDR}e"  // or RequestHeader set Client-IP "%{REMOTE_ADDR}e"
    [...]
