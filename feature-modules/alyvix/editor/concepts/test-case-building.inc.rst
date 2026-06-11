

When you build a test case, you're creating a set of visual templates that can be matched
against your app interfaces in real time, allowing Alyvix to interact with those interfaces
just like a person would.

When you use Alyvix to build test cases that work with your applications, you'll spend most
of your time creating those test cases and their test case objects, so it's worth some time
to learn how to do it the right way and understand from the beginning what the available
options are.

There are three main tools for building test cases:

.. rst-class:: bignums-xl

#. **Alyvix Designer:**  Define individual test case objects to be dynamically matched
   against the visual components of your app's interface, and assign GUI actions that
   are carried out when those matches are detected
#. **Alyvix Selector:**  Easily manipulate (inspect, copy, edit, delete) large numbers of test
   case objects created with Designer, as well as view, filter and sort their properties
#. **Alyvix Editor:**  Create scripts using test case objects (as sequential execution units,
   conditionals, and loops) to interact with any app according to complex behaviors you define

For almost all cases, Alyvix Editor (which includes Designer and Selector) is all you'll need.

Once you've built a test case, you can let it interact with your chosen app either by using
`Alyvix Robot <https://alyvix.com/learn/test_case_execution.html#test-case-execution-top>`__, or testing it directly within Editor.

The `Test Case Data Format <https://alyvix.com/learn/test_case_data_format.html#test-case-data-format-top>`__ page provides technical details on how
Alyvix test case files are organized and what they contain, while the
`Getting Started <https://alyvix.com/learn/getting_started.html#getting-started-top>`__ section and official
`YouTube channel <https://www.youtube.com/@alyvix8723>`_
include detailed mini-tutorials and topic-based videos on how to use Alyvix Editor and Robot.

All of the Alyvix applications can be launched from the Windows Command Prompt or PowerShell.
Note that they inherit the permissions of the shell they were launched from.

.. _test_case_building_designer:

.. rubric:: The Designer Panel

Use Alyvix Designer to define the relevant parts of your application's interface at a certain
step in the overall task. Designer lets you select graphic elements on a captured screen to
use as *test case objects*, which can be images, rectangles, or text. You can then add actions
that are applied when those objects are recognized later during a live interaction with the
application.

Designer bundles this set of graphic elements and actions, each called a *component*, as a
single *test case object*, which you can then use as a building block to compose more
complicated behaviors with scripts using Alyvix Editor.

.. _test_case_building_designer_launch:

The following sections of the guide present further information on Alyvix Designer:

* The :ref:`Designer Interface Overview <alyvix_designer_interface_overview>` page provides a
  high-level overview and describes the general layout of the Designer panel
* The :ref:`Component Tree <alyvix_designer_component_tree_top>` page describes the specific
  types of graphical elements available, including what they can do and how they can interact
  with the application interface
* You can find more detailed information about the available options for test case objects and
  components on the :ref:`Interface Options <alyvix_designer_options>` page

Although it's typically used under Alyvix Editor, Designer can be run in standalone
mode as follows:

.. code-block:: doscon
   :class: short-code-block

   C:\Alyvix\testcases> alyvix_designer

with the following command line options:

+---------------+-------+----------+----------------------------------------------+
| Option        | Alias | Argument | Description                                  |
+---------------+-------+----------+----------------------------------------------+
| -\ -delay     | -d    | *<n>*    | Wait *n* seconds before grabbing the screen, |
|               |       |          | giving you time to move windows around       |
+---------------+-------+----------+----------------------------------------------+
| -\ -filename  | -f    | *<name>* | Supply the file name with no extension       |
+---------------+-------+----------+----------------------------------------------+
| -\ -object    | -o    | *<name>* | Supply the Object name                       |
+---------------+-------+----------+----------------------------------------------+
| -\ -verbose   | -v    | *<n>*    | Set the verbosity level for debugging output |
|               |       |          | ranging from **0** (min) to **2** (max)      |
+---------------+-------+----------+----------------------------------------------+

|



.. _test_case_building_selector:

.. rubric:: The Selector Panel

Use Alyvix Selector to :ref:`centralize the management <alyvix_selector_interface_top>`
of all of your test case objects created with Designer, such as copying test cases objects
to other Alyvix files, and set *monitoring* parameters like warning, critical and timeout
values.  It also provides options to view, edit and delete test cases objects.

.. _test_case_building_selector_launch:

Selector is organized as a set of tabbed panels representing one or more test case files, and
a list of test case objects within each tab.  This allows you to quickly switch between them and
filter, search and edit within a single file, or copy test case objects across opened test case
files.

Although it's typically used under Alyvix Editor, Selector can be run in standalone
mode as follows:

.. code-block:: doscon
   :class: short-code-block

   C:\Alyvix\testcases> alyvix_selector

with the following command line options:

+---------------+-------+----------+----------------------------------------------+
| Option        | Alias | Argument | Description                                  |
+---------------+-------+----------+----------------------------------------------+
| -\ -filename  | -f    | *<name>* | Supply the file name with no extension       |
+---------------+-------+----------+----------------------------------------------+
| -\ -verbose   | -v    | *<n>*    | Set the verbosity level for debugging output |
|               |       |          | ranging from **0** (min) to **2** (max)      |
+---------------+-------+----------+----------------------------------------------+

|



.. _test_case_building_editor:

.. rubric:: Alyvix Editor

Alyvix Editor helps you create scripts consisting of individual test case objects created
with Designer and Selector. These scripts allows you to create complex interactions using
the test case objects in a specific order. And it saves everything -- scripts, test case
objects, and monitoring parameters -- in a single :file:`.alyvix` test case file.

.. _test_case_building_editor_launch:

The following sections of the guide present further information about Alyvix Editor:

* The :ref:`Editor Interface Overview <alyvix_editor_interface_top>` page describes the layout of
  the various panels and how to use the interface controls
* The :ref:`Scripting Management <alyvix_editor_script_mgmt_top>` page describes the various
  types of scripts and their what they're used for
* The :ref:`Scripting Panel <alyvix_editor_scripting_panel_top>` page explains how to create
  individual scripts composed of test case objects

Alyvix Editor can be run as follows:

.. code-block:: doscon
   :class: short-code-block

   C:\Alyvix\testcases> alyvix_editor

with the following command line options:

+---------------+-------+----------+-----------------------------------------------+
| Option        | Alias | Argument | Description                                   |
+---------------+-------+----------+-----------------------------------------------+
| -\ -filename  | -f    | *<name>* | The test case file name (with no extension)   |
+---------------+-------+----------+-----------------------------------------------+
| -\ -verbose   | -v    | *<n>*    | Sets the verbosity level for debugging output |
|               |       |          | ranging from **0** (min) to **2** (max)       |
+---------------+-------+----------+-----------------------------------------------+

|
