.. _neteye-module-install-single-node:

Single Node
-----------

The procedure to install additional |ne| components is divided into
three steps, the second of which requires to run different commands
depending on the type of the component.

#. The first step is to :ref:`update the NetEye single instance
   <neteye-update-single>`, during which all bug-fixes are installed
   and the list of packages updated.

#. Then, install the |ne| Component. Depending on the type of
   component, use one of the following commands.

   * **NetEye Module**

     Take the appropriate *Yum group name* from the :ref:`NetEye Modules table
     <neteye-modules>` and run:

     .. code:: bash

        neteye# dnf -y groupinstall {yum-group-name} --enablerepo=neteye

   * **Beta Software**

     Before installing packages from the **neteye-beta repository**, it is
     required to enable it with command

     .. code:: bash

        neteye# dnf -y install neteye-testing --enablerepo=neteye

     Next, find the package name using the command shown in Section
     :ref:`beta-software`, then issue the following command to install it.

     .. code:: bash

        neteye# dnf -y install {package_name}-{version} --enablerepo=neteye-beta

#. As last step, run :command:`neteye install`

Once done, please follow the directions given in section
:ref:`access-new-module`, to complete the overall installation.
