If the |ne| Elastic Stack module is installed:

1. The `rubygems.org <https://rubygems.org/>`_ domain should be reachable by the |ne| Master only during the
   update/upgrade procedure. This domain is needed to update additional Logstash plugins and thus is
   required only if you manually installed any Logstash plugin that is not present by default.
2. There is a number of configuration items that should not be modified in order to avoid issues during the update/upgrade of your instance.
   Please check out :ref:`untouchable-settings` for details.
