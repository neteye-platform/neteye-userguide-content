
Each upgraded package can potentially create .rpmsave and/or .rpmnew
files. You will need to verify and migrate all such files.

You can find more detailed information about what those files are and
why they are generated in the `official RPM documentation
<http://ftp.rpm.org/max-rpm/ch-rpm-upgrade.html>`_.

Briefly, if a configuration file has changed since the last version,
**and** the configuration file was edited since the last version, then
the package manager will do one of these two things:

-  If the new system configuration file should replace the edited
   version, it will save the old edited version as an .rpmsave file and
   install the new system configuration file.
-  If the new system configuration file should not replace the edited
   version, it will leave the edited version alone and save the new
   system configuration file as an .rpmnew file.

You can use the following commands to locate .rpmsave and .rpmnew files::

    # updatedb
    # locate *.rpmsave*
    # locate *.rpmnew*

The instructions below will show you how to keep your customized
operating system configurations.

.. rubric:: How to Migrate an .rpmnew Configuration File

The update process creates an .rpmnew file if a configuration file has
changed since the last version so that customized settings are not
replaced automatically. Those customizations need to be migrated into
the new .rpmnew configuration file in order to activate the new
configuration settings from the new package, while maintaining the
previous customized settings. The following procedure uses Elasticsearch
as an example.

First, run a diff between the original file and the .rpmnew file::

   # diff -uN /etc/sysconfig/elasticsearch /etc/sysconfig/elasticsearch.rpmnew

OR

::

   # vimdiff /etc/sysconfig/elasticsearch /etc/sysconfig/elasticsearch.rpmnew

Copy all custom settings from the original into the .rpmnew file. Then
create a backup of the original file::

   # cp /etc/sysconfig/elasticsearch /etc/sysconfig/elasticsearch.01012018.bak

And then substitute the original file with the .rpmnew::

   # mv /etc/sysconfig/elasticsearch.rpmnew /etc/sysconfig/elasticsearch

.. rubric:: How to Migrate an .rpmsave Configuration File

The update process creates an .rpmsave file if a configuration file has
been changed in the past and the updater has automatically replaced
customized settings to activate new configurations immediately. In order
to preserve your customizations from the previous version, you will need
to migrate those from the original .rpmsave into the new configuration
file.

Run a diff between the new file and the .rpmsave file::

   # diff -uN /etc/sysconfig/elasticsearch.rpmsave /etc/sysconfig/elasticsearch

OR

::

   # vimdiff /etc/sysconfig/elasticsearch.rpmsave /etc/sysconfig/elasticsearch

Copy all custom settings from the .rpmsave into the new configuration
file, and preserve the original .rpmsave file under a different name::

   # mv /etc/sysconfig/elasticsearch.rpmsave /etc/sysconfig/elasticsearch.01012018.bak
