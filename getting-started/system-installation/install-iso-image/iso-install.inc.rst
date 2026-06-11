.. _install-hw:

Install on Physical Server
~~~~~~~~~~~~~~~~~~~~~~~~~~

To install |ne| on a physical server, either burn the ISO on a DVD-Rom
or to a USB stick, then turn the server on and follow the instruction
in the next sections to install a :ref:`Single Node and Satellite
<single-node-install>`, a :ref:`Cluster Node <cluster-node-install>`:
in case of a Satellite, follow the additional direction in Section
:ref:`satellite-node-install`.

.. _neteye_hypervisors:

Install on VMware
~~~~~~~~~~~~~~~~~

To create a virtual machine and install |ne|, start VMware
Workstation, click :menuselection:`File / New Virtual Machine`, follow
these steps, and click :guilabel:`Next` whenever necessary.

#. Select **Custom (advanced)** and leave the defaults as they are
#. Select **ISO image** and then the |ne| ISO you want to install. You
   might see the warning *Could not detect which operating system is
   in this image. You will need to specify which operating system will
   be installed*: simply ignore it.
#. Select *Linux* as the **Guest OS**, and specify *Red Hat Linux* in
   the dropdown menu
#. Name the VM as you prefer, and select the location to store it
#. Specify the number of processors
#. Specify the amount of memory
#. Select the type of connection according to your needs
#. Select *VMware Paravirtual* as **SCSI controller**
#. Select *SATA* or *SCSI* as virtual disk type
#. Select *Create a new virtual disk*
#. Specify the disk capacity
#. Rename the disk to a name you prefer
#. Review the configuration you just created, deselect **Automatically
   start the VM**, and click  **Finish**

You can now proceed to section :ref:`powering-up-vm`.

Install on KVM
~~~~~~~~~~~~~~

To create a KVM virtual machine and install |ne|, start the Virtual
Machine Manager, click :menuselection:`File / New Virtual Machine`,
follow these steps, and click :guilabel:`Forward` whenever necessary.

#. Select **Local install media**
#. Choose the |ne| ISO to install, uncheck **Automatically detect from
   the installation media/source** under **Choose the operating system you
   are installing**, and then select **RedHat 8** for the OS

   .. hint:: You can also start typing in the text box to see the
      available OSs, or run *osinfo-query os* in your terminal to see
      all available variants).

#. Specify the amount of memory and the number of processors
#. Specify the disk capacity
#. Give the VM the name you prefer, review the configuration and
   untick **Customize configuration before install**
#. In the configuration panel that appears, go to **Boot Options** and
   check that *Disk1* and *CDRom* are both selected
#. In the next configuration panel that appears, go to **VirIO Disk 1**,
   expand the Advanced options, and change the disk bus to SATA
#. Click on **Apply** to propagate your changes
#. Click on **Begin installation** to start the |ne| installation

You can now proceed to section :ref:`powering-up-vm`.

Install on HyperV
~~~~~~~~~~~~~~~~~

To create an HyperV virtual machine and install |ne| on it, start
Hyper-V Manager, select :menuselection:`File / New Virtual Machine`,
follow these steps, and click :guilabel:`Next` whenever necessary.

#. Specify the name of your new VM and where to store it
#. Leave the defaults for **Specify Generation**
#. Specify the amount of memory
#. Select **Default switch** as the connection adapter
#. Specify the disk capacity
#. Specify the ISO that you want to install
#. Review your settings, then click :guilabel:`Finish`
#. Before firing your new VM up, look at the list of startup media in
   the BIOS settings. Be sure that the CD is in the list
#. Click :menuselection:`Action / Start` to start the virtual machine

You should now proceed to section :ref:`powering-up-vm`.

.. _powering-up-vm:

Powering up the VM
~~~~~~~~~~~~~~~~~~

At this point, your VM has been successfully created, and you can
power it up. After a few seconds, the |ne| logo will appear, and a
countdown to automatically initiate the installation will start.

After ten seconds, if no key is pressed, the installation process
starts. The installation process will take several minutes to complete,
after which the VM will reboot from the internal hard disk.

At the end of the boot process, you will be prompted to enter your
credentials (root/admin). If the login is successful, you can now
start to :ref:`configure your NetEye VM <neteye-initial-conf>`.
