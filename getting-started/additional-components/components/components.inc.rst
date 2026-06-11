
.. _neteye-components:

Additional |ne| Components
--------------------------

Additional |ne| Components are organized in the following categories:

**(NetEye) Feature Modules**
   Fully fledged modules, whose functionalities are well defined
   and established. A Feature Module corresponds to a *yum group*
   which contains all the required packages to make the module
   working.

**Preview Software**
   Not yet mature |ne| Modules which provide a set of
   functionalities that can change in the future; they might be
   installed to try new software that will be later become part of the
   official |ne| platform.

**Beta Software**
   Applications that belong to a |ne| Module, which have not yet
   reached the level of stability of |ne| Modules. They are
   suitable for early adopters to test latest functionalities but it
   is not suggested to install them on production environment.  Unlike
   Feature Modules and Preview Software, Beta Software usually is not
   a full module but a subset of packages of an existing module.

Feature Modules, Preview Software, and Beta Software belong to
different repositories--``neteye``, ``neteye-extras``, and
``neteye-beta`` respectively--and can be installed from the command
line. The installation procedure is described in the following sections.

.. _neteye-modules:

|ne| Feature Modules
~~~~~~~~~~~~~~~~~~~~~~

All |ne| Feature Modules belong to the ``neteye`` repository.

.. csv-table::
   :header: "Module", "Requires", "Yum group name"

   "Alyvix", "|ne| Core", "neteye-alyvix"
   "Asset", "|ne| Core", "neteye-asset"
   "Command Orchestrator", "|ne| Core", "neteye-cmd"
   "ntopng", "|ne| Core", "neteye-ntopng"
   "Elastic Stack", "|ne| Core", "neteye-elastic-stack"
   "SLM", "|ne| Core", "neteye-slm"
   "Tools", "|ne| Core", "neteye-tools"
   "vSphereDB", "|ne| Core", "neteye-vmd"


.. note:: Please remember that, due to the possibly large amount of space required by Elasticsearch,
   it is strongly recommended to create a logical volume dedicated to it when installing the |ne| Elastic Stack Feature Module.

.. _neteye-modules_installation:

|ne| Feature Modules Installation
"""""""""""""""""""""""""""""""""

To install a |ne| Feature Module, you can apply the following procedure:

#. On one node, run the command to add the feature module:

   .. code:: bash

      neteye# neteye feature-module add {feature_module_name}

#. If on a cluster:

   #. Review the templates for the services that you can find in
      :file:`/etc/neteye-services.d/<feature_module_name>/`
      and edit the options as needed.

   #. Run the command to install cluster services for the feature module:

      .. code:: bash

         neteye# neteye cluster install

#. Perform the standard |ne| installation procedure:

   .. code:: bash

      neteye# neteye install

.. _neteye-modules_licenses:

|ne| Feature Modules Licenses
"""""""""""""""""""""""""""""

As an Elastic OEM partner, Würth IT Italy provides the Elastic license
with the |ne| Elastic Stack Feature Module. The license provides a fully functional Elastic Stack,
with all features of the `Platinum subscription <https://www.elastic.co/subscriptions>`__, covering also, for example
APM functionalities.
Although the Elastic Stack Platinum license is the default option, it is possible to upgrade the license
plan to the `Enterprise subscription <https://www.elastic.co/subscriptions>`__ using the
:ref:`dedicated command<neteye-feature-module-neteye-elastic-stack-subscription-set>`.

.. _beta-software:

Beta Software
~~~~~~~~~~~~~

Beta software resides in the ``neteye-beta`` repository. Unlike other
|ne| repositories, this repository may include multiple and
unrelated packages, and possibly multiple versions of a same
package. It is therefore possible to install even a single package
from this repository; the following command lists all packages
available there and allows to check which one to install.

.. code:: bash

   neteye# dnf list available --disablerepo=* --enablerepo=neteye-beta

The output to this command contains a list of packages and their
version, for example:

.. container:: codeblock

   .. code::

      monitoring-plugins-debuginfo.x86_64    2.3.1_neteye1.2.0-1    neteye-beta

Here, **monitoring-plugins-debuginfo** is the {package_name} of the
package and **2.3.1_neteye1.2.0-1** its {version}. Both data are
required if you want to install a specific version of a package.
