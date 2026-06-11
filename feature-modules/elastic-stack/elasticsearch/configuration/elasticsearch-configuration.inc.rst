Elasticsearch settings need to be added to their
configuration files at run-time.

Starting from NetEye 4.16 release, configuration files for Elasticsearch
are not anymore modified by :command:`neteye install`. The
support of the run-time configuration is instead done via
`environment <https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html#_environment_variable_substitution>`__.

The default values for NetEye are stored in the
:file:`/neteye/local/elasticsearch/conf/sysconfig/elasticsearch` file and they can be overridden by creating the
:file:`/neteye/local/elasticsearch/conf/sysconfig/elasticsearch-user-customization` file and specify the new values.

By restarting Elasticsearch, the new settings are now loaded at run-time,
thus overriding the default ones.

Elasticsearch temporary directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| uses the :file:`/neteye/local/elasticsearch/data/tmp` directory as the
temporary storage for Elasticsearch. It is essential to ensure that this
directory resides on a filesystem that does **not** have the :command:`noexec`
mount option enabled. This directory shall be changed to a different location if
the default one can not meet the requirements, by setting the
:command:`ES_TMPDIR` environment variable in the user customization file.
