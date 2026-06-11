.. _neteye-log-analytics:

NetEye Log Analytics
~~~~~~~~~~~~~~~~~~~~

The log management functionality granted by SIEM
is reflected in **Log Analytics** module of your NetEye installation.
Log Analytics serves as platform for analysis and visualization of SIEM
processes, and is designed to work with Elasticsearch.
You can use it to search, view, and interact with data stored in Elasticsearch
indices, then easily perform advanced data analysis and visualize your
data in a variety of charts, tables, and maps.

To support the configuration of datasources, NetEye comes with a
preconfigured `Elasticsearch Data Source` called *Elasticsearch-Logstash*,
which points to the Logstash indices present on Elasticsearch. This
Data Source can be used to build Grafana dashboards, similarly to what
is done in the :ref:`ITOA module <itoa-module-description>`.

This Elasticsearch-Logstash data source uses the **grafana** user's TLS
certificates (generated during NetEye installation) to authenticate
X-Pack Security. The **grafana** user by default has no permissions on
any Elasticsearch index, but you can add necessary permissions from
within X-Pack Security, just by mapping its role to the user
**grafana**.

You can also change the configuration of the Elasticsearch-Logstash data
source should you need to personalize it, but be aware that if you
change its name, a new data source named Elasticsearch-Logstash source
will be regenerated the next time :command:`neteye install` is run.


Configuring access Roles
````````````````````````

The permissions and roles must be configured as :ref:`described in the
Authentication section <roles-users-permissions>`.

.. note:: Kibana Users who are managed by NetEye will be overwritten
   at each login.

   If you modify such a user using the Kibana admin panel, those
   changes will be lost!

User Management
+++++++++++++++

Elasticsearch user management is based on three main entities:

* **User**: The authenticated user is defined by a username and
  password and should be assigned to a role. The users will be
  automatically created during the NetEye login.
* **Role mapping**: in NetEye this is used for granting access to all
  those users that are authenticated via certificates. You can check
  roles in Elastic in the dedicated :ref:`Elasticsearch Access Control
  <elasticsearch-access-control>`
  section.
* **Role**: A named set of permissions that translate to privileges on
  resources. A more detailed description of how the user authorization
  works in Elasticsearch can be found at the `following link
  <https://www.elastic.co/guide/en/elasticsearch/reference/7.17/authorization.html>`__
  Elasticsearch provides some **built-in roles** you can explicitly
  assign to users. You can always refer to the official documentation
  for delving `into the topic
  <https://www.elastic.co/guide/en/elasticsearch/reference/7.17/built-in-roles.html>`__.

A complete guide on how to create new roles within Elasticsearch can
be found on Elasticsearch official documentation about `Defining roles
<https://www.elastic.co/guide/en/elasticsearch/reference/7.17/defining-roles.html>`__.

Each NetEye Role can be mapped to one or more Kibana roles.

If a user belongs to more than one NetEye Role with different Kibana
roles, the same mapping will be reflected within Kibana.

The Kibana module adds a ``kibana/roles`` field for each role (i.e., a
comma-separated list of kibana roles which must be correctly defined in
Kibana) and also a new role ``neteye_kibana_sso``, which allows to carry
out operations on tokens of Elasticsearch Token Service.

All the users with **Administrative Access** role in NetEye or belonging
to a NetEye role that set **Full module access** for Kibana, will be by
default mapped with all the following built-in Kibana roles:

* **kibana_admin**
* **superuser**

With the introduction of X-Pack security in Elasticsearch, additional
roles are available to allow communication with other modules: they
are described in :ref:`Elasticsearch Access Control
<elasticsearch-access-control>`.
