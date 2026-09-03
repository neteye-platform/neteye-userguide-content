
Before installing Alyvix, first check that your environment meets the system requirements.
If it does, you will then need to install Python on your Windows machine before installing
Alyvix itself.


.. _system_requirements_top:

System Requirements
```````````````````

.. note::

   Alyvix assumes that you have **one virtual or physical machine** exclusively dedicated to
   running Alyvix test cases.

You should check that your designated machine and the account on that machine meet the following
requirements before you install Alyvix:

+---------------------+--------------------------------------------------------------------------+
|                     | Minimum                                                                  |
+---------------------+--------------------------------------------------------------------------+
| :file:`Operating`   | **Windows 10 (64-bit) Pro or Enterprise**                                |
| :file:`System`      | -or- **Windows Server 2016, 2019 or 2022**                               |
|                     | (32-bit versions of Windows are :warn:`not` compatible                   |
|                     | with Alyvix)                                                             |
+---------------------+--------------------------------------------------------------------------+
| :file:`Processor`   | 2 CPUs                                                                   |
+---------------------+--------------------------------------------------------------------------+
| :file:`Memory`      | 4GB RAM                                                                  |
+---------------------+--------------------------------------------------------------------------+
| :file:`Graphics`    | 24-bit RGB or 32-bit RGBA screen color depth                             |
+---------------------+--------------------------------------------------------------------------+
| :file:`Application` | Users defined on Alyvix Server must have the proper permissions          |
| :file:`Permissions` | to run and interact with the application interface being monitored       |
+---------------------+--------------------------------------------------------------------------+

|


.. _installation_versions:

Versions
````````
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix Version |   Python Version Required    |       Install Command                                                                            |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.8.0   | |python-download-link-397|   | ``pip install alyvix --extra-index-url https://alyvix.github.io/alyvix-python-packages/simple/`` |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.7.1   | |python-download-link-397|   | ``pip install alyvix==3.7.1``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.7.0   | |python-download-link-397|   | ``pip install alyvix==3.7.0``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.6.0   | |python-download-link-397|   | ``pip install alyvix==3.6.0``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.5.0   | |python-download-link-397|   | ``pip install alyvix==3.5.0``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.4.0   | |python-download-link-397|   | ``pip install alyvix==3.4.0``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.3.2   | |python-download-link-397|   | ``pip install alyvix==3.3.2``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.3.1   | |python-download-link-397|   | ``pip install alyvix==3.3.1``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.2.3   | |python-download-link-397|   | ``pip install alyvix==3.2.3``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.2.0   | |python-download-link-397|   | ``pip install alyvix==3.2.0``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+
| Alyvix 3.1.6   | |python-download-link-379|   | ``pip install alyvix==3.1.6``                                                                    |
+----------------+------------------------------+--------------------------------------------------------------------------------------------------+

|


.. _install_release_python_install:


Installing Python
`````````````````

Follow this procedure to install Python on your system:


#. Download the |python-download-link|.
   Note that Alyvix is :warn:`not` compatible with 32-bit versions of Python.

#. Start the installation:

   1. :warn:`Right-click` on the downloaded executable and select **"Run as administrator"**
   2. Check the box **"Add Python 3.9 to PATH"** at the bottom
   3. Choose the option **"Customize installation"**

   .. image:: img/python-install-01.png
      :class: image-boxshadow image-very-large zoomable-image
      :alt: The first panel of the python installation process.

#. On the second panel all the option boxes should already be checked.  Click **"Next"** to
   continue.

   .. image:: img/python-install-02.png
      :class: image-boxshadow image-very-large zoomable-image
      :alt: The second panel of the python installation process.

#. On the third panel, make sure the advanced options are set as shown below.  Then under
   **"Customize install location"**, insert the recommended location :file:`C:\\Python39\\`.
   Finally, click on **"Install"**.  At this point Python will begin installing, typically
   requiring about 5 minutes.

   .. image:: img/python-install-03.png
      :class: image-boxshadow image-very-large zoomable-image
      :alt: The third panel of the python installation process.

#. The installation is correct if the following command returns a version number rather than an
   error message (if not, you will need to edit your ``Path`` environment variable):

   .. code-block:: doscon
      :class: tiny-code-block

      C:\> python --version
      Python 3.9.7



.. _install_release_alyvix_install:


Installing Alyvix
`````````````````

Alyvix itself is installed via *pip*, the official Python package manager.  *pip* will
automatically detect that your system is running Python 3 and will install Alyvix 3,
placing all executables in the directory
:file:`C:\\Python39\\Lib\\site-packages\\alyvix\\`.

The Alyvix installer is launched from the command prompt that, like the Python installer, must be
**started in Administrator mode**.  Only this single step is required:

.. rst-class:: bignums

#. Run the following command to download and install the latest Alyvix master release along
   with all of its dependencies:

   .. code-block:: doscon
      :class: tiny-code-block

      C:\> pip install alyvix

   If necessary, you can install Alyvix through a proxy by adding a parameter:

   .. code-block:: doscon
      :class: long-code-block

      C:\> pip install --proxy=https://[user:password@]proxyserver:port alyvix

#. Check that the installation concluded successfully.  Run the following command:

   .. code-block:: doscon
      :class: tiny-code-block

      C:\> alyvix_editor

   If the Alyvix Editor window appears as shown here, then everything is working properly.

   .. image:: img/ae_full_screen.png
      :class: image-boxshadow image-very-large zoomable-image
      :alt: The main Alyvix Editor screen indicating the installation was successful.


We recommend you place your Alyvix test cases in a separate data directory reserved for test cases,
such as :file:`C:\\Alyvix\\Testcases\\`.



.. _install_upgrade:

Upgrading Alyvix
````````````````

Only two steps are needed to upgrade to the latest version:

#. Close Alyvix

   * Save all of your currently open projects
   * Close all Alyvix user interfaces and applications
   * Terminate all running Alyvix processes

#. Upgrade Alyvix with *pip*

   * Start a Command Prompt **in administrator mode**
   * Follow the case below that corresponds to your situation in order to download and upgrade
     to the latest Alyvix master release along with all of its dependencies:

     * If you have Python 3.7.9 installed, uninstall it, then manually remove its
       containing folder (e.g., ``C:\Python37\``) and re-install Alyvix with *pip* as shown in
       the section :ref:`Installing Alyvix <install_release_alyvix_install>` above.

     * If you have Python 3.9.7 installed, then you can use the *pip --upgrade* command:

       .. code-block:: doscon
          :class: short-code-block

          C:\> pip install --upgrade alyvix



.. _install_offline_install:

Offline Installation
````````````````````

In order to create an offline installation, you will need two machines:

* An internet-connected *installer* machine with a working Python 3 installation to serve as a
  source for downloading the required software packages
* A second *target* machine that will serve as the offline probe

The following steps will then enable you to install Alyvix on the target machine:

.. rst-class:: bignums

#. On the *installer* Windows **64-bit** machine with an existing **Python 3** installation:

   * Create a new directory with appropriate permissions
   * Download the |python-download-link| to that directory
   * Download the Alyvix application and its dependencies with this command:

     .. code-block:: doscon
        :class: short-code-block

        C:\...\MyFolder> pip download alyvix

#. Copy the entire directory onto the target machine

#. Use the Python installer now on the target machine to
   :ref:`install Python following the instructions above <install_release_python_install>`

#. Open a command prompt in the directory on the target machine and install Alyvix with this
   command:

   .. code-block:: doscon
      :class: code-block

      C:\...\MyFolder> pip install alyvix-3.2.0-cp39-cp39-win_amd64.whl --no-index --find-links .

#. Check that the installation was successful by starting Alyvix Editor:

   .. code-block:: doscon
      :class: tiny-code-block

      C:\> alyvix_editor

   If the :ref:`Editor window <alyvix_editor_interface_top>` appears, the installation was
   successful.



.. _install_uninstall:

Uninstalling Alyvix and Python
``````````````````````````````

To uninstall Alyvix, regardless of whether you also want to uninstall Python, run the following
command in a Command Prompt or Power Shell **with Administrator privileges**:

.. code-block:: doscon

   C:\> pip uninstall alyvix

Pip will list the packages to be removed, and then ask for confirmation.

If you only use Python to run Alyvix, you can now also remove Python itself.  Go to
**Start Menu > Windows Settings > Apps**, scroll down, and remove both |python-remove-name| and
:file:`Python Launcher`.  The process may take up to 5 minutes.


Once completed, you can also manually remove the :file:`C:\\Python39\\` directory (or whichever
directory you specified during installation) and either archive or delete your test case
directory.

The Python installer does not remove environment variables, so if desired you can manually
remove them at **System Properties > Environment Variables > System Variables > Path**.



.. _install_troubleshooting:

Installation Troubleshooting
````````````````````````````

Below are some potential installation problems and their solutions.

.. admonition::  "Python" command does nothing or launches Microsoft Store on Windows 10

   This error occurs when during :ref:`installation steps #2 and #4 <install_release_python_install>`
   you forgot to check the boxes to add Python to the path and environment variables.  Typing the
   :command:`python` command in the command prompt under this condition will launch Microsoft Store
   in an attempt to install it that way.

   To correct this situation you will need to either (1) uninstall and then reinstall Python, or
   (2) manually add the appropriate paths in the **System Properties > Environment Variables** panel.


.. admonition::  "ImportError: DLL load failed" --- Failure to Import Tesseract OCR

   This error is caused by a missing dependency of the Tesseract OCR module, which requires that the
   file :file:`vcomp140.dll` be present in Windows during installation by *pip*:

   .. code-block:: doscon
      :class: nocopy

      File "C:\Python39\lib\site-packages\alyvix\core\tesserocr\__init__.py", line 1, in <module> from ._tesserocr import *
      ImportError: DLL load failed: The specified module could not be found.

   You can fix this problem by installing the **Microsoft Visual C++ Redistributable for Visual Studio**,
   which contains the required file.  It's available at
   `https://aka.ms/vs/16/release/vc_redist.x64.exe <https://aka.ms/vs/16/release/vc_redist.x64.exe>`_.


.. admonition::  "WindowsError: [Error 206] The filename or extension is too long"

   This error is caused by Python's *pip* command attempting to write temporary files using a very long,
   automatically generated, unique path name.

   .. code-block:: doscon
      :class: nocopy

      WindowsError: [Error 206] The filename or extension is too long: <path/file>

   You can fix this problem by editing the Windows setting that allows file paths longer than 260 characters.
   Open the Group Policy editor (GPEDIT.MSC) and navigate to **Computer Configuration > Administrative
   Templates > System > Filesystem**.  At the right, double click on *Enable Win32 long paths*, then in
   the popup panel click on the "Enabled" setting.  Click on *Apply* and *OK*, then you'll need to
   restart your computer or VM.
