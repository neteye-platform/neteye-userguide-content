.. _nodes_behind_proxy:

Nodes behind a Proxy
--------------------

Some software installed on the NetEye Nodes needs to have access to resources from the Internet.
In certain environments though the NetEye Nodes do not have direct Internet access and they need
instead to pass through a proxy which forwards the requests to the wider web. In these cases you
need configure some parts of neteye so that the software can access the proxy.

Assume your NetEye Nodes need to pass through a proxy which has the settings below:

#. The proxy hostname is **proxy.example.com**
#. The proxy listens on port **12345**
#. The proxy uses basic authentication and valid credentials are:

   #. username: **myuser**
   #. password: **mypassword**

.. _nodes_behind_proxy_configuring_the_environment:

Configuring the environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To add the proxy configuration for software like curl, wget, python, etc. the environment
variables need to be set accordingly. The variables should set in the environment by
creating the file :file:`/etc/profile.d/neteye-proxy.sh` and adding the following lines:

.. code:: bash

   #!/usr/bin/env bash

   export http_proxy=http://<proxy_username>:<proxy_password>@<proxy_ip>:<proxy_port>
   export https_proxy=https://<proxy_username>:<proxy_password>@<proxy_ip>:<proxy_port>
   export no_proxy=localhost,127.0.0.1,127.0.0.2,::1,neteyelocal # (in a cluster:,<cluster_node_ips>)

If the node is part of a cluster you also need to add the IPs of each node in the cluster, so that the
internal cluster traffic is not sent across the proxy. Be aware that ``no_proxy`` does not support
network masks, so the IPs need to be added individually.

To refresh the current session run :command:`source /etc/profile.d/neteye-proxy.sh` afterwards.

With the configuration mentioned above, when the node is not part of a cluster, the file could look like this:

.. code:: bash

   #!/usr/bin/env bash

   export http_proxy=http://myuser:mypassword@proxy.example.com:12345
   export https_proxy=https://myuser:mypassword@proxy.example.com:12345
   export no_proxy=localhost,127.0.0.1,127.0.0.2,::1,neteyelocal

Configuring Subscription Manager to use the proxy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is an example of how to set the proxy directives in the Subscription Manager configuration.
These commands modify the :command:`/etc/rhsm/rhsm.conf` file.

.. code:: bash

   # subscription-manager config --server.proxy_hostname "proxy.example.com"
   # subscription-manager config --server.proxy_scheme "http"
   # subscription-manager config --server.proxy_port "12345"

The following commands are needed only if the proxy is protected by authentication.

.. code:: bash

   # subscription-manager config --server.proxy_user "myuser"
   # subscription-manager config --server.proxy_password "mypassword"

Configuring DNF
~~~~~~~~~~~~~~~

Configure *DNF* to pass through the proxy. To do this add your proxy settings to the ``[main]``
section of the file :file:`/etc/dnf/dnf.conf`. The resulting file should be similar to the one
below (refer to the `DNF manual <https://dnf.readthedocs.io/en/latest/conf_ref.html>`_ for more
rinformation on the :file:`dnf.conf` options):

.. code:: yaml

  [main]
  gpgcheck=1
  installonly_limit=3
  clean_requirements_on_remove=True
  best=True
  skip_if_unavailable=False
  proxy="http://proxy.example.com:12345"
  proxy_username="myuser"
  proxy_password="mypassword"

Configuring Kibana
~~~~~~~~~~~~~~~~~~

If you have installed the :ref:`Elastic Stack module <elastic-stack>`, your proxy configuration must be
provided to the Kibana service for it to reach the :code:`epr.elastic.co` repository in order to handle Elastic Agents'
integrations. To accomplish this, you can append the following line to the Kibana service’s configuration file at
:file:`/neteye/shared/kibana/conf/kibana.yml`:

.. code:: yaml

  xpack.fleet.registryProxyUrl: "<protocol>://<proxy_username>:<proxy_password>@<proxy_ip>:<proxy_port>"

.. note::

   If you are in a cluster environment, please apply this change on the node in which the `kibana` service is running.

After making this change, if your Kibana instance was already configured you must restart the service in order to apply
the new settings as shown below; otherwise this operation will be taken care of by the :command:`neteye install`
command.

On a Single Node environment:

.. code:: bash

   # systemctl restart kibana-logmanager

On a Cluster environment:

.. code:: bash

   # pcs resource restart kibana

Configuring Elasticsearch plugins
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you need to install additional Elasticsearch plugins via the :command:`elasticsearch-plugin` utility, or also
if you already installed them, you must configure the proxy settings in the Elasticsearch configuration.
To do so, you need to add the following lines to the file :file:`/etc/profile.d/neteye-proxy.sh`:

.. code:: bash

   export CLI_JAVA_OPTS="-Djava.net.useSystemProxies=true"


Alternatively, you can configure specific proxy settings for Elasticsearch by adding the following lines to the
:file:`/etc/profile.d/neteye-proxy.sh` file:

.. code:: bash

   export CLI_JAVA_OPTS="-Dhttp.proxyHost=<proxy_ip> -Dhttp.proxyPort=<proxy_port> -Dhttps.proxyHost=<proxy_ip> -Dhttps.proxyPort=<proxy_port>"


You can find more information about available options in the `Elasticsearch documentation
<https://www.elastic.co/guide/en/elasticsearch/plugins/current/_other_command_line_parameters.html#_proxy_settings>`_.

.. warning::

   Due to a limitation in the Elasticsearch plugin manager, it is not possible to configure an authenticated proxy for
   the plugin installation process. If you are behind a proxy that requires authentication, you are required to either
   configure your http proxy to allow the Elasticsearch nodes to reach the Internet without authentication, or
   manually download the plugin and install it. Note that, if you choose the latter option, you will need to manually
   uninstall it during the :command:`neteye update` or :command:`neteye upgrade` process and reinstall it
   afterwards.
