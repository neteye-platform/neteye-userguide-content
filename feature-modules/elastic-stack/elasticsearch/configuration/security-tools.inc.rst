
Elasticsearch security helper tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The secure communication provided by the X-Pack Security requires
additional parameters such as authentication certificates to interact
with the Elastic Stack APIs. We have developed a few helper tools, based
on curl, to simplify your interaction with the APIs.

The **Elasticsearch helper script** lets you omit all the authentication
parameters for the admin user, which would otherwise be required.

Location: **/usr/share/neteye/elasticsearch/scripts/es\_curl.sh**

The **NetEye helper script** can be used instead if you only need read
permission for the fields *@timestamp* and *host* on the Logstash index
entries. This script is used by NetEye for self-monitoring activities.

Location:
**/usr/share/neteye/elasticsearch/scripts/es\_neteye\_curl.sh**
