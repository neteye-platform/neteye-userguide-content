.. _additional-software-installation:

Additional Software Installation
--------------------------------

To satisfy particular use cases you may have the necessity to use software
that is not pre-installed on the NetEye Nodes. To ensure that this software
and all its dependencies are automatically managed by the system, please
install and manage it only via the *DNF* RPM package manager.

.. warning::
   It is **strongly recommended** to avoid installing software in any other way.

   For example, installing a Python module via *pip* will not let the system
   manage the module and keep track of its dependencies during updates,
   which may lead to the module being outdated and its dependencies being broken.
   In this case, instead of installing the Python module via *pip*, please find an
   RPM package that provides the module and install it via *DNF*.
