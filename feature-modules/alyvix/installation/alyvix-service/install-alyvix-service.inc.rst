

Before installing Alyvix Service, first check that your setup meets the system requirements.

Note that Alyvix Service also requires Alyvix to be installed on the same machine.
Alyvix, also known as *Alyvix Core* to distinguish it from *Alyvix Service*,
is a free and open source engine for designing and building GUI test cases that show what
a task and its interface should look like and how they behave, and then later running them
autonomously on the local Windows machine.


.. _system_requirements_alyvix_service:

System Requirements
```````````````````

.. note::

   Alyvix Service assumes that you have one virtual or physical machine exclusively dedicated to running
   Alyvix test cases.

You should check that your designated machine and the account on that machine meet the following
requirements before you install Alyvix Service:

+---------------------+--------------------------------+-----------------------------------------+
|                     | Minimum                        | Recommended                             |
+---------------------+--------------------------------+-----------------------------------------+
| :file:`Operating`   | **Windows 10 (64-bit)**        | **Windows Server 2016, 2019 or 2022**   |
| :file:`System`      | **Pro or Enterprise**          | (English language)                      |
|                     +--------------------------------+-----------------------------------------+
|                     | (32-bit versions of Windows are :warn:`not` compatible with Alyvix       |
|                     | Service)                                                                 |
+---------------------+--------------------------------+-----------------------------------------+
| :file:`Processor`   | 2 CPUs                         | 2 CPUs base **+** 2 CPUs per session    |
+---------------------+--------------------------------+-----------------------------------------+
| :file:`Memory`      | 4GB RAM                        | 4GB RAM base **+** 4GB RAM per session  |
+---------------------+--------------------------------+-----------------------------------------+
| :file:`Graphics`    | 24-bit RGB or 32-bit RGBA screen color depth                             |
+---------------------+--------------------------------+-----------------------------------------+
| :file:`Remote`      | Users defined on Alyvix Service must have RDP access (through RDC        |
| :file:`Desktop`     | *mstsc.exe*) to the machine itself (e.g. the user must be a Remote       |
|                     | Desktop User, and the firewall must not be set to block local RDC)       |
|                     +--------------------------------+-----------------------------------------+
|                     | **1 session only:** No Windows | **Multiple sessions in parallel:**      |
|                     | Terminal Server available;     | Windows Terminal Server allows          |
|                     | 1 test case executed at a time | multiple test cases to run at once      |
+---------------------+--------------------------------+-----------------------------------------+
| :file:`Application` | Users defined on Alyvix Service must have the proper permissions         |
| :file:`Permissions` | to run and interact with the application interface being monitored       |
+---------------------+--------------------------------+-----------------------------------------+

|


.. _installation_versions_alyvix_service:

Versions
````````

+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service Version            | Required Alyvix Core Version     | PostgreSQL Version              | Alyvix API Version |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.8.x              | |link-to-alyvix-install37x|      | |link-postgresql-install-12.x|  | 3,4,5,6            |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.7.x              | |link-to-alyvix-install37x|      | |link-postgresql-install-12.x|  | 3, 4, 5            |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.6.x              | |link-to-alyvix-install36x|      | |link-postgresql-install-12.x|  | 3, 4               |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.5.x              | |link-to-alyvix-install36x|      | |link-postgresql-install-12.x|  | 0, 1, 2, 3         |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.4.x              | |link-to-alyvix-install35x|      | |link-postgresql-install-12.x|  | 0, 1, 2            |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.3.x              | |link-to-alyvix-install35x|      | |link-postgresql-install-12.x|  | 0, 1               |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.2.x              | |link-to-alyvix-install35x|      | |link-postgresql-install-12.x|  | 0, 1               |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.1.x              | |link-to-alyvix-install34x|      | |link-postgresql-install-12.x|  | 0, 1               |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+
| Alyvix Service 2.0.x              | |link-to-alyvix-install33x|      | |link-postgresql-install-12.x|  | 0, 1               |
+-----------------------------------+----------------------------------+---------------------------------+--------------------+

|


.. _installation_steps_alyvix_service:

Installation Steps
``````````````````

The following steps will install Alyvix Service on your machine:

#. **Request an Alyvix Service subscription**

   Choose your `preferred subscription plan <https://alyvix.com/service#plans>`_ and get in touch with us
   `to request it <https://alyvix.com/team>`_, providing a machine IP from where you will download the
   software package.  You'll obtain access to `our repository <https://repo.wuerth-phoenix.com/alyvix-service/>`_.

#. **Install Alyvix Core**

   Follow `the installation instructions <https://alyvix.com/learn/getting_started/install.html>`_
   for Python and Alyvix.

#. **Install PostgreSQL**

   Download and run the most recent version of
   `the PostgreSQL 12.X installer <https://www.enterprisedb.com/downloads/postgres-postgresql-downloads>`_
   for the **Windows x86-64** architecture.  Be sure to run it :warn:`in administrator mode`. |br|

   Click "Next" to accept all the defaults until it asks you to set the password.  Change the
   default password to ensure the security of your system, and make a note of it so that
   you can configure Alyvix Service to use PostGre in the next step below.

   .. image:: /feature-modules/alyvix/installation/alyvix-service/img/postgre-install-05.png
      :width: 70%
      :align: center
      :alt: Use a secure password and remember it.

   Continue clicking "Next" to accept the remaining defaults and complete the installation.

#. **Install Alyvix Service**

   Download the most recent version of the installer (:file:`alyvix_service_<version>.zip`) from
   `the repository <https://repo.wuerth-phoenix.com/alyvix-service/>`_, and run the :file:`setup.exe`
   installer which can be found inside the .zip file :warn:`in administrator mode`. |br|
   Set the database password from step #3 as follows:

   * Open the file |config-file-location| :warn:`in administrator mode`
   * Paste the password in this line: |br1|
     ``"database":{.. "password": "<your_password>", ..}``

#. **Mandatory security configuration**

   First save your HTTPS certificate files used for browser security as follows:

   * Create the folder :file:`C:\\ProgramData\\Alyvix\\certs\\webserver\\`
   * Save :file:`cert.crt` as an HTTPS certificate recognized by your CA
   * Save :file:`cert.key` as its (unprotected) password

   Note that the private key is all you need, you should not be asked for an additional password.

   Next, copy the JSON Web Token (JWT), which is used for API authentication purposes,
   from your monitoring system to Alyvix Service.

   * Create the folder :file:`C:\\ProgramData\\Alyvix\\certs\\jwt\\`
   * Copy the JWT certificate file from your monitoring system into the folder above,
     renaming it to :file:`public.pem`.

#. **Start Alyvix Service**

   Run **Alyvix Service** within Windows Services **Task Manager > Services Tab > Alyvix Service > Start**

   .. image:: /feature-modules/alyvix/installation/alyvix-service/img/service_alyvix_restart.png
      :width: 70%
      :align: center
      :alt: Start the Alyvix Service.

#. **Monitoring system integration**

   At this point Alyvix Service is installed and running, and you can now proceed to integrate it
   :ref:`installing the NetEye-Alyvix module <neteye-modules>` (`neteye-alyvix`) and then :ref:`configuring how it's used within NetEye <monitoring_integrations_neteye_checklist>`.

|


.. _install_upgrade_alyvix_service:

Upgrading
`````````

The following steps will upgrade Alyvix Service to the latest version on your machine:

#. Back up your existing configuration

   * If you're upgrading from version 2.4.x or earlier, create the subdirectory :file:`webserver\\`
     within :file:`C:\\ProgramData\\Alyvix\\certs\\` and move :file:`cert.crt` and :file:`cert.key`
     to the new subdirectory (before version 2.6.0 these files may be in :file:`C:\\Program Files\\Alyvix\\Alyvix Service\\`)
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

   Follow `the instructions here <https://alyvix.com/learn/getting_started/install.html#upgrading-alyvix>`_

#. Install the new version of Alyvix Service

   * Run the Alyvix Service Installer (:file:`setup.exe`) found in the Alyvix Service package
   * Restore the two backup files and the directory you made in step #1:

     * |config-file-location|
     * |mapping-file-location|
     * |security-directory-location|

#. Run Alyvix Service

   * Start Alyvix Service:  **Windows Services > Alyvix Service > start**
   * Sign out of the current session

|


.. _uninstallation_steps:

Uninstalling Alyvix Service
```````````````````````````

The following steps will remove Alyvix Service from your machine.  Basically you will need to reverse
the steps performed during installation.

#. Disable the relevant Alyvix Nodes within your integrated monitoring system.

#. Stop Alyvix Service under the Services tree:
   **Start > Computer Management > Services and Applications > Services > Alyvix Service**

#. Uninstall Alyvix Service:
   **Start > Settings > Apps > Alyvix Service > Uninstall** |br|
   If desired, also uninstall PostgreSQL the same way.

#. Remove these two directories:

   * :file:`C:\\Program Files\\Alyvix\\`
   * :file:`C:\\Program Data\\Alyvix\\`

#. If desired, remove Alyvix Core and/or Python using
   `the Alyvix uninstall instructions <https://alyvix.com/learn/getting_started/install.html#uninstalling-alyvix-and-python>`_.
