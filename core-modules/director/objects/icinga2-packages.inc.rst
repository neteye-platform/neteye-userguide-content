.. _icinga2-packages:

Icinga2 packages
````````````````

Icinga2 packages are provided for most Linux distributions and windows.
You can see a comprehensive list of supported OSs following this link
`repo.wuerth-phoenix.com/icinga2-agents/ <https://repo.wuerth-phoenix.com/icinga2-agents/>`__
and selecting your NetEye version.

For some enterprise operating systems, for example RHEL and SLES, the agent is only available for customers with an
active subscription, and is hence only downloadable by registered IP addresses, just like our product packages.
These agents are available via the `/subscription/` URL.


.. hint::
   Please refer to the consultants, or |support|_ in case of doubt about your IP authentication.


.. note::
   In order to install Icinga2 packages you need to have the ``boost`` libraries installed
   (version 1.66.0 or newer) or available via the default package manager.

Icinga2 repository versioning
+++++++++++++++++++++++++++++

You must use Icinga2 packages provided by the NetEye repositories instead of
the official Icinga2 packages. From **4.16** onwards, icinga2 agents are version
specific both for the NetEye version and for the monitored operating system version.
You can modify package URLs accordingly. If you are downloading
packages for 4.<neteye_minor>, you need to change **/icinga2-agents/neteye-x.x** with
/icinga2-agents/neteye-4.<neteye_minor> in below packages urls.

Add the NetEye repository for Icinga2 packages
++++++++++++++++++++++++++++++++++++++++++++++

This section will explain how to add the dedicated NetEye repository for
Icinga2 packages in different OSs and distributions (e.g. Ubuntu, CentOS, SUSE),
thus supporting the installation of an Icinga2 agent via the default
package manager installed in the OS.

URL repository follow this syntax::

    https://repo.wuerth-phoenix.com/icinga2-agents/neteye-4.<neteye_minor>/<distribution>-<codename_or_version>/

Icinga2 RPM repository
++++++++++++++++++++++

To add the repository that provides the Icinga2 RPM packages (e.g. CentOS, SUSE, Fedora)
you have to add a new repository definition to your system.

Let us suppose that you need to add the new repository definition on a **CentOS 7**
machine, which is monitored via **NetEye 4.xx**. You can add the repo definition
in a file :file:`neteye-icinga2-agent.repo`::

    [neteye-agent]
    name=NetEye Icinga2 Agent Packages
    baseurl=https://repo.wuerth-phoenix.com/icinga2-agents/neteye-4.xx/centos-7/
    gpgcheck=0
    enabled=1
    priority=1

Please note that the location of this file will change according with
the distribution used.  For example, on Fedora and CentOS
installations the default repo definition directory is
:file:`/etc/yum.repos.d/`, while SUSE will use
:file:`/etc/zypp/repos.d/`.

Once the new repository has been added, you need to load the new
repository data by running :command:`dnf update`.

Icinga2 DEB repository
++++++++++++++++++++++

To add the Icinga2 agent repository on Ubuntu or Debian systems you have to create the file
:file:`neteye-icinga2-agent.list` in the directory :file:`/etc/apt/sources.list.d/``.

For example, to add the repository on a Ubuntu 20.04 Focal Fossa you have to create
a file with the following content::

    "deb [trusted=yes] https://repo.wuerth-phoenix.com/icinga2-agents/neteye-4.xx/ubuntu-focal/ stable main"

Finally, run :command:`apt update` to update the repo data.

Icinga2 windows packages
++++++++++++++++++++++++

Get the Icinga2 Agent for Windows accessing the URL below and
downloading the .msi file (replace **x.x** with the NetEye version
number, e.g. *4.31*)::

    https://repo.wuerth-phoenix.com/icinga2-agents/neteye-x.x/windows/

Install Icinga2
+++++++++++++++

To install Icinga2, follow `Icinga2
Documentation <https://icinga.com/docs/icinga-2/latest/doc/02-installation/>`__
Icinga2 requires boost libraries to work properly. Ensure that the
libraries are also installed on the system.

To install windows msi on agent, follow `Icinga2 Windows Agent
Installation <https://icinga.com/docs/icinga-2/latest/doc/06-distributed-monitoring/#agent-setup-on-windows>`__
official document.
