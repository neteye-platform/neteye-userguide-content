
Before installing Alyvix Service, first check that your setup meets the system requirements.

.. _alyvix_service_system_requirements:

System Requirements
```````````````````

Alyvix Service assumes that you have one virtual or physical machine exclusively
dedicated to running Alyvix test cases, with *Alyvix Core* already installed.
You should check that each of these designated machines meets all the
requirements here before installing Alyvix Service:

.. table::
   :widths: 24 38 38

   +------------------------------+--------------------------------+-----------------------------------------+
   |                              | **Minimum**                    | **Recommended**                         |
   +------------------------------+--------------------------------+-----------------------------------------+
   | **Operating System**         | **Windows 10 (64-bit)**        | **Windows Server 2016, 2019 or 2022**   |
   |                              | **Pro or Enterprise**          | (English language)                      |
   |                              +--------------------------------+-----------------------------------------+
   |                              | (32-bit versions of Windows are :file:`not` compatible with              |
   |                              | Alyvix Service)                                                          |
   +------------------------------+--------------------------------+-----------------------------------------+
   | **Processor**                | 2 CPUs                         | 2 CPUs base **+** 2 CPUs per session    |
   +------------------------------+--------------------------------+-----------------------------------------+
   | **Memory**                   | 4GB RAM                        | 4GB RAM base **+** 4GB RAM per session  |
   +------------------------------+--------------------------------+-----------------------------------------+
   | **Graphics**                 | 24-bit RGB or 32-bit RGBA screen color depth                             |
   +------------------------------+--------------------------------+-----------------------------------------+
   | **Remote Desktop**           | Users defined on Alyvix Service must have RDP access (through RDC        |
   |                              | *mstsc.exe*) to the machine itself (e.g. the user must be a Remote       |
   |                              | Desktop User, and the firewall must not be set to block local RDC)       |
   +------------------------------+--------------------------------+-----------------------------------------+
   |                              | **1 session only:** No Windows | **Multiple sessions in parallel:**      |
   |                              | Terminal Server available;     | Windows Terminal Server allows          |
   |                              | 1 test case executed at a time | multiple test cases to run at once      |
   +------------------------------+--------------------------------+-----------------------------------------+
   | **Application Permissions**  | Users defined on Alyvix Service must have the proper permissions         |
   |                              | to run and interact with the application interface being monitored       |
   +------------------------------+--------------------------------+-----------------------------------------+

|


.. _alyvix_service_installation_versions:

Versions
````````

Ensure that the Alyvix Service version you want to install is compatible with
the installed versions of Alyvix Core and PostgreSQL.

.. table::
   :widths: 30 25 25 20

   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | **Alyvix Service Version**        | **Required Alyvix Core Version**                         | **PostgreSQL Version**          | **Alyvix API Version** |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.9.x              | :ref:`Alyvix 3.8.x <alyvix_core_installation_versions>`  | |link-postgresql-install-18.x|  | 3,4,5,6                |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.8.x              | :ref:`Alyvix 3.7.x <alyvix_core_installation_versions>`  | |link-postgresql-install-18.x|  | 3,4,5,6                |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.7.x              | :ref:`Alyvix 3.7.x <alyvix_core_installation_versions>`  | |link-postgresql-install-18.x|  | 3, 4, 5                |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.6.x              | :ref:`Alyvix 3.6.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 3, 4                   |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.5.x              | :ref:`Alyvix 3.6.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 0, 1, 2, 3             |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.4.x              | :ref:`Alyvix 3.5.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 0, 1, 2                |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.3.x              | :ref:`Alyvix 3.5.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 0, 1                   |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.2.x              | :ref:`Alyvix 3.5.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 0, 1                   |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.1.x              | :ref:`Alyvix 3.4.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 0, 1                   |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+
   | Alyvix Service 2.0.x              | :ref:`Alyvix 3.3.x <alyvix_core_installation_versions>`  | |link-postgresql-install-12.x|  | 0, 1                   |
   |                                   |                                                          | |alyvix-ext-link-icon|          |                        |
   +-----------------------------------+----------------------------------------------------------+---------------------------------+------------------------+

|


.. _alyvix_service_installation_steps:

Installation Steps
``````````````````

The following steps will install Alyvix Service on your machine:

#. **Request an Alyvix Service subscription**

   Choose your `preferred subscription plan <https://alyvix.com/service#plans>`_ and get in touch with us
   `to request it <https://alyvix.com/team>`_, providing a machine IP from where you will download the
   software package.  You'll obtain access to
   `our repository <https://repo.wuerth-phoenix.com/alyvix-service/>`_. |br|

#. **Install Alyvix Core**

   Follow :ref:`the installation instructions <install_alyvix_core>`
   for Python and Alyvix. |br|

#. **Install PostgreSQL**

   Download and run the most recent version of
   `the PostgreSQL 12.X installer <https://www.enterprisedb.com/downloads/postgres-postgresql-downloads>`_
   for the **Windows x86-64** architecture.  Be sure to run it :file:`in administrator mode`.

   Click "Next" to accept all the defaults until it asks you to set the password.  Change the
   default password to ensure the security of your system, and make a note of it so that
   you can configure Alyvix Service to use PostGre in the next step below.

   .. image:: /feature-modules/alyvix/installation/alyvix-service/img/postgre-install-05.png
      :width: 70%
      :align: center
      :alt: Use a secure password and remember it.

   Continue clicking "Next" to accept the remaining defaults and complete the installation. |br|

#. **Install Alyvix Service**

   Download the most recent version of the installer (:file:`alyvix_service_<version>.zip`) from
   `the repository <https://repo.wuerth-phoenix.com/alyvix-service/>`_, and run the :file:`setup.exe`
   installer which can be found inside the .zip file :file:`in administrator mode`. |br|

   .. note:: See how to
      :ref:`alyvix_service_unblock_alyvix_files` in case Windows blocks a downloaded Alyvix Service file.

   Set the database password from step #3 as follows:

   * Open the file |config-file-location| :file:`in administrator mode`
   * Paste the password into this line: |br1|
     ``"database":{.. "password": "<your_password>", ..}`` |br|

#. **Mandatory security configuration**

   First save your HTTPS certificate files used for browser security as follows:

   * Create the folder :file:`C:\\ProgramData\\Alyvix\\certs\\webserver\\`
   * Save :file:`cert.crt` as an HTTPS certificate recognized by your CA
   * Save :file:`cert.key` as the unencrypted private key corresponding to :file:`cert.crt`

   Note that the private key is all you need, you should not be asked for an additional password.

   Next, copy the JSON Web Token (JWT), which is used for API authentication purposes,
   from your monitoring system to Alyvix Service.

   * Create the folder :file:`C:\\ProgramData\\Alyvix\\certs\\jwt\\`
   * Copy the JWT certificate file from your monitoring system into the folder above,
     renaming it to :file:`public.pem`. |br|

#. **Start Alyvix Service**

   Run **Alyvix Service** within Windows Services **Task Manager > Services Tab > Alyvix Service > Start**

   .. image:: /feature-modules/alyvix/installation/alyvix-service/img/service_alyvix_restart.png
      :width: 70%
      :align: center
      :alt: Start the Alyvix Service.

   |

#. **Monitoring system integration**

   At this point Alyvix Service is installed and running, and you can now proceed to integrate it
   :ref:`installing the NetEye-Alyvix module <neteye-modules>` (`neteye-alyvix`) and then :ref:`configuring how it's used within NetEye <monitoring_integrations_neteye_checklist>`.

|


.. _alyvix_service_install_upgrade:

Upgrading
`````````

The following steps will upgrade Alyvix Service to the latest version on your machine:

#. Back up your existing configuration

   * If you're upgrading from version 2.4.x or earlier, create the subdirectory :file:`webserver\\`
     within :file:`C:\\ProgramData\\Alyvix\\certs\\` and move :file:`cert.crt` and :file:`cert.key`
     to the new subdirectory (before version 2.5.0 these files may be in :file:`C:\\Program Files\\Alyvix\\Alyvix Service\\`)
   * Now back up the entire security certificate directory: |br1|  |security-directory-location|
   * Then back up your Alyvix Service configuration file: |br1|  |config-file-location|
   * And back up your Alyvix Service tenant roles file: |br1|  |mapping-file-location|

#. Uninstall the current version of Alyvix Service

   * Stop Alyvix Service:  **Windows Services > Alyvix Service > Stop**
   * Close all Alyvix Client windows (where appropriate)
   * Uninstall Alyvix Service:  **Windows Control Panel > Programs and Features > Alyvix Service > Uninstall**
   * Remove residual Alyvix Service files (when appropriate):  :file:`C:\\ProgramData\\Alyvix\\`
     (:file:`C:\\Program Files\\Alyvix\\Alyvix Service\\` for versions before 2.6.0)
   * Remove old Alyvix Client scheduled tasks:  **Windows Task Scheduler > alyvix_client<..> > delete**

#. Upgrade Alyvix Core

   Follow :ref:`the instructions here <alyvix_core_install_upgrade>` |br|

#. Install the new version of Alyvix Service

   * Run the Alyvix Service Installer (:file:`setup.exe`) found in the Alyvix Service package
   * Restore the two backup files and the directory you made in step #1:

     * |config-file-location|
     * |mapping-file-location|
     * |security-directory-location|

#. Run Alyvix Service:

   * Start Alyvix Service:  **Windows Services > Alyvix Service > start**
   * Sign out of the current session

|


.. _alyvix_service_uninstallation_steps:

Uninstalling Alyvix Service
```````````````````````````

The following steps will remove Alyvix Service from your machine.  Basically you will need to reverse
the steps performed during installation.

#. Disable the relevant Alyvix Nodes within your integrated monitoring system

#. Stop Alyvix Service under the Services tree:
   **Start > Computer Management > Services and Applications > Services > Alyvix Service**

#. Uninstall Alyvix Service:
   **Start > Settings > Apps > Alyvix Service > Uninstall** --
   if desired, also uninstall PostgreSQL the same way.

#. Remove these two directories:

   * :file:`C:\\Program Files\\Alyvix\\`
   * :file:`C:\\ProgramData\\Alyvix\\`

#. If desired, remove Alyvix Core and/or Python using
   :ref:`the Alyvix uninstall instructions <alyvix_core_install_uninstall>`

|


.. _alyvix_service_unblock_alyvix_files:

Unblock Alyvix Service Files
````````````````````````````

When Alyvix Service installation files are downloaded from the Internet, Windows
may mark them with security information known as the Mark of the Web (MOTW).
In some environments, this can prevent the installer or related files from opening correctly.

If Windows blocks a downloaded Alyvix Service file, the following message may be displayed::

   This file came from another computer and might be blocked to help protect this computer.

Before starting the Alyvix Service installation, verify that the downloaded files are not blocked.
Windows may mark files downloaded from the Internet with security information known as the Mark of the Web (MOTW).
Windows can also propagate this mark from a downloaded ZIP archive to the files extracted from it,
which can leave the installer or its dependencies blocked.

Before extracting the Alyvix Service archive, unblock the downloaded ``alyvix_service_<version>.zip`` file.

To unblock the downloaded ZIP file:

#. Open **File Explorer**.
#. Locate the downloaded ``alyvix_service_<version>.zip`` file.
#. Right-click the ZIP file and select **Properties**.
#. On the **General** tab, check the **Security** section at the bottom of the window.
#. If the **Unblock** option is available, select it.
#. Select **Apply**, then select **OK**.
#. Extract the unblocked ``alyvix_service_<version>.zip`` archive.
#. Open the extracted folder and run ``setup.exe``.

.. warning::

   Unblock only files downloaded from trusted sources.

If Windows continues to display security warnings for trusted Alyvix Service files:

* Make sure Windows and all required applications are up to date.
* Verify that the files were downloaded from an official or trusted source.
* Re-download the files if they may have been corrupted.
* Check whether your organization applies security policies that block downloaded files.
* Temporarily disable third-party download managers or security software only for testing purposes.

|


.. _alyvix_service_install_troubleshooting:

Installation Troubleshooting
````````````````````````````

Below are some potential installation problems and their solutions.

.. admonition::  My test case runs, but no reports appear

    If the monitoring system allows you to configure a test case, and you can
    see that the test case is being scheduled and is running correctly, double
    check in Alyvix Editor that it has at least one step with the
    `"Measure" flag <alyvix_selector_interface_top>`_
    checked.

    If no measurement boxes checked, then no measurements will be sent to the
    monitoring system, and its report generator will thus not have any data to
    create a report with.
