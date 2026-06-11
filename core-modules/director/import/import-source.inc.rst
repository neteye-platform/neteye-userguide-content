.. _fileshipper-module:

Use Text Files as an Import Source
``````````````````````````````````

The FileShipper interface allows you to import objects like hosts, users
and groups from plain-text file formats like CSV and JSON.

The documentation below assumes that you are :ref:`already familiar
with Importing and Synchronization in Director
<automating-imports>`. Before using FileShipper, please be sure that
the module is ready by:

- Enabling it in **Configuration > Modules > fileshipper**.
- Creating paths for both the configuration and the files::

    $ mkdir /neteye/shared/icingaweb2/conf/modules/fileshipper/
    $ mkdir /data/file-import

  And then defining a source path for those files within the following
  configuration file::

    $ cat > /neteye/shared/icingaweb2/conf/modules/fileshipper/imports.ini
    [NetEye File import]
    basedir = "/data/file-import"

.. _fileshipper-import:

Adding a new Import Source
++++++++++++++++++++++++++

From **Director > Import data sources**, click on the “Add” action,
then enter a name and description for this import source. For “Source
Type”, choose the “Import from files (fileshipper)” option as in
:numref:`figure-fileshipper-add-source`. The form will then expand to
include several additional options.

.. _figure-fileshipper-add-source:

.. figure::  /core-modules/director/img/fileshipper-import02.png
   :alt: Add a Fileshipper Import Source

   Add a Fileshipper Import Source

Choose a File Format
++++++++++++++++++++

Next, enter the name of the principal index column from the file, and
choose your desired file type from **File Format** as in
:numref:`figure-fileshipper-choose-file-format`.

.. _figure-fileshipper-choose-file-format:

.. figure::  /core-modules/director/img/fileshipper-import03.png
   :alt: Choose a File Format

   Choosing the File Format.

If you would like to learn more about the supported file formats,
please read the :ref:`file format documentation
<fileshipper-file-formats>`.

Select the Directory and File(s)
++++++++++++++++++++++++++++++++

You will now be asked to choose a **Base Directory**
(:numref:`figure-fileshipper-choose-basedir`).

.. _figure-fileshipper-choose-basedir:

.. figure::  /core-modules/director/img/fileshipper-import04.png
   :alt: Choose a Base Directory

   Choosing the Base Directory.

The FileShipper module doesn't allow you to freely choose any file on
your system. You must provide a safe set of base directories in
Fileshipper's configuration directory as described in the first section
above. You can include additional directories if you wish by creating
each directory, and then modifying the configuration file, for instance:

.. code:: ini

   [NetEye CSV File Import]
   basedir = "/data/file-import/csv"

   [NetEye XSLX File Import]
   basedir = "/data/file-import/xslx"

Now you are ready to choose a specific file
(:numref:`figure-fileshipper-choose-file`).

.. _figure-fileshipper-choose-file:

.. figure::  /core-modules/director/img/fileshipper-import05.png
   :alt: Choose a specific file

   Choosing a specific file or files.

.. note:: For some use-cases it might also be quite useful to import
   all files in a given directory at once.

Once you have selected the file(s), press the “Add” button. You will
then see two additional parameters to fill for the CSV files: the
delimiter character and field enclosure character
(:numref:`figure-fileshipper-add-parameters`). After filling them out,
you will need to press the “Add” button a second time.

.. _figure-fileshipper-add-parameters:

.. figure::  /core-modules/director/img/fileshipper-import06.png
   :alt: Add extra parameters

   Add extra parameters.

The new synchronization rule will now appear in the list
(:numref:`figure-fileshipper-added-source`).  Since you have not used
it yet, it will be prefixed by a black question mark.

.. _figure-fileshipper-added-source:

.. figure::  /core-modules/director/img/fileshipper-import07.png
   :alt: The newly added import source

   The newly added import source.

Now follow the steps for **importing** at the page on :ref:`Importing
and Synchronization in Director <automating-imports>`. Once complete,
you can then look at the Preview panel of the Import Source to check
that the CSV formatting was correctly recognized. For instance, given
this CSV file::

  dnshostname,displayname,OS
  ne4north1.company.com,NE4 North Building 1,Windows
  ne4north2.company.com,NE4 North Building 2,Linux

then :numref:`figure-fileshipper-csv-preview` shows the following preview:

.. _figure-fileshipper-csv-preview:

.. figure::  /core-modules/director/img/fileshipper-import08.png
   :alt: CSV preview

   Previewing the results of CSV import.

If the preview is correct, then you can proceed to Synchronization, or
set up a Job to synchronize on a regular basis.
