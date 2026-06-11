.. _elastic_beat_agent_installation:

Installation and Configuration of Beat Agents
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before being able to take fully advantage of the Beat feature, agents
must be installed on the monitored hosts, along with the necessary
certificates. On the hosts, any kind of Beat can be installed; for
example, the Winlogbeat is available from `the official download
page <https://www.elastic.co/downloads/beats/winlogbeat>`__;
`installation
instructions <https://www.elastic.co/guide/en/beats/winlogbeat/7.17/winlogbeat-installation-configuration.html>`__
are available as well. The agent configuration is stored in the YAML
configuration file ``winlogbeat.yml``. A description of the options
available in the Beat's configuration file can be found in the `official
documentation <https://www.elastic.co/guide/en/beats/winlogbeat/7.17/configuring-howto-winlogbeat.html>`__.

.. note:: You need to install a Beat whose version is compatible with
   the Elastic version installed on NetEye, which is
   7.17. To find out which version of Beat you can
   install, please check `the compatibility matrix
   <https://www.elastic.co/support/matrix/#matrix_compatibility>`__

Relevant to the configuration are the following options:

-  **ignore_older**, which indicates how many hours/days it should
   gather data from. By default, indeed, the Beat collects **all** the
   data it finds, meaning it can act retroactively. This is the
   **default option** if not specified, so make sure to properly
   configure this option, to not overload the initial import of data and
   to avoid potential problems like crash of Logstash and ES disk space.
   ​
-  **index: ”winlogbeat”**, which is needed to match NetEye's templates
   and ILM.
