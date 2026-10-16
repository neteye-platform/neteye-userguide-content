
Alyvix test cases are stored as JSON files with an  ``.alyvix`` extension,
and represent the unique mechanism for interchange between Alyvix
:ref:`tools <alyvix_test_case_building_top>`. Each file is self-contained,
without any external dependencies.

Alyvix Core and its data structures are open source. The entire JSON file format
of an Alyvix test case file is described in detail below.

|


.. _alyvix_test_case_data_format_file:

Test Case Files
```````````````

Each time you exit :ref:`Designer <alyvix_designer_interface_overview>` or
:ref:`Selector <alyvix_selector_interface_top>`, or save an object in
:ref:`Editor <alyvix_editor_interface_top>`, the JSON object representing the
test case is saved in the current directory under the file name:

:file:`<filename>.alyvix`

where ``filename`` is the value of the ``-f`` parameter passed when starting
Editor or Designer.

When you call Alyvix Robot :ref:`via the command line <alyvix_robot_result_cli>`
with that file as a parameter, it will create a new data file based
on the original file name, but with the resulting measures included. Appended
to the file name is the timestamp at the moment of execution:

:file:`<filename>_<full-timestamp>.alyvix`

For example:

:file:`logintest_20191220_145228_UTC+0100.alyvix`

.. note::

   The timestamped file is not produced when a test case is executed within
   Editor via ``Run``, only when executed directly from Alyvix Robot, since
   *storing* the results is only useful in an automated monitoring scenario
   where the results may need to be used later.

.. todo: |runblue|

|


.. _alyvix_test_case_data_format_json:

Test Case JSON Structure
````````````````````````

A single ``.alyvix`` file stores the entire test case, and a timestamped file
additionally contains all measures resulting from its single execution run.

The following example JSON structure illustrates the high-level structure of
the test case object:

.. code-block:: json
   :linenos:

   {
      "maps": { },
      "objects": {
         "<test-case-object-name>": {
            "call": { },
            "components": {
               "<resolution>": {
                  "groups": [
                     { "main": { },
                       "subs": [ ] },
                     { "main": { },
                       "subs": [ ] },
                     { "main": { },
                       "subs": [ ] }
                  ],
                  "screen": "<base64>"
               }
            },
            "date_modified": "<timestamp>",
            "detection": {
               "break": true,
               "timeout_s": 10,
               "type": "appear" },
            "measure": {
               "group": "<transaction-group-name>",
               "output": true,
               "series": { },
               "thresholds": { } }
         }
      },
      "script": {
         "case": [
            "<test-case-object-name>",
            { "disable": "<test-case-object-name>" },
            { "flow": "<test-case-object-name>",
              "if-true": "<test-case-object-name>" },
            { "flow": "<test-case-object-name>",
              "if-false": "<test-case-object-name>" },
            { "flow": "<test-case-object-name>",
              "for": "<test-case-map-name>" }
         ],
         "sections": {
            "<section-name>": [
               "<test-case-object-name>"
            ],
            "exit": [ ],
            "fail": [ ] }
      }
   }

The individual sections making up the JSON structure are explained below.

|


.. _alyvix_test_case_data_format_description:

*Top Level*
===========

The top level corresponds to the principal elements of Alyvix Editor:

.. code-block:: json

   { "maps": {
        "map-name":  { } },
     "objects": {
        "<test-case-object-name>":  { } },
     "script": {
        "case":  [ ],
        "sections":  { } }
   }

#. ``maps`` --  An ordered set of values that a script can loop over, for
   instance to insert a sequence of values in multiple text fields.
#. ``objects``  -- A list of the individual test case objects created with
   Alyvix Designer. Each ``test case object`` is identified uniquely in the
   list by its object name.
#. ``script``  --  The scripts created for a test case, both the main
   script and any section (subroutine) scripts, in the order they are executed.

.. todo: :bolditalic:`maps` ...

|


*Maps*
======

Each named map under the :bolditalic:`maps` field contains a set of named keys
(rows) and a fixed number of cell values, to represent an **m x n** matrix
(columns cannot be named):

.. code-block:: json

   { "<map-name-1>": {
        "<key-name-1>": [ "key1-val1",
                          "key1-val2" ],
        "<key-name-2>": [ "key2-val1",
                          "key2-val2" ] }
   }

When arguments are passed to Alyvix Robot in the
:ref:`command line interface <alyvix_robot_cli_launch>`, a new map structure
will be added under the name ``cli``, and
:ref:`containing the arguments as keys <alyvix_designer_options_strings_map_cli>`.
Because of this, the map name ``cli`` is reserved.

.. todo: :bolditalic:`cli` ...

|


*Objects*
=========

The value of each property contained within the ``objects`` substructure (shown
here as "<test-case-object-name>") corresponds to a test case object created in
Designer/Editor. Those values have the following meanings:

.. todo: :bolditalic:`objects`

.. code-block:: json

   { "<test-case-object-name>": {
        "call": { },
        "components": { },
        "date_modified": "<timestamp>",
        "detection": { },
        "measure": { }
     }
   }

#. ``call``  --  The recorded options to start or kill an external application
   when a test case object is :ref:`first called <alyvix_designer_options_components_root>`
#. ``components``  --  The representation of the :ref:`component tree <alyvix_designer_component_tree_top>`
#. ``date_modified``  --  The time the test case object was last modified
   (:ref:`also displayed in Selector <alyvix_selector_interface_screenshot>`)
#. ``detection``  --  The test case object's
   :ref:`detection conditions <alyvix_designer_options_test_case_object>`
   such as *timeout*, *break*, and *appear/disappear*
#. ``measure``  --  Details of the last execution via Alyvix Robot
   (:ref:`see below <alyvix_test_case_data_format_measure>`) if this is a
   timestamped ``.alyvix`` file, along with the settings for transaction group,
   output, and thresholds.

.. todo: :bolditalic:`call` ...

|


.. _alyvix_test_case_data_format_components:

*Objects > {object} > Components*
=================================

The *components* section represents the
:ref:`component tree <alyvix_designer_component_tree_top>`
of a given test case object. The example below shows how you can have a
different component tree at each distinct screen resolution.

.. code-block:: json

   { "components": {
       "<resolution>": {
           "groups": [
               { "main": {
                   "detection": { },
                   "interactions": { },
                   "visuals": { } },
                 "subs": [ ] },
               { "main": { },
                 "subs": [ ] },
               { "main": { },
                 "subs": [ ] } ],
           "screen": "<base64>" } }
   }

``<resolution>``  --  The resolution of a screen capture as shown in
:ref:`Alyvix Selector <alyvix_selector_interface_top>`. This value is
not selected by the user, but depends on the hardware and the settings in
use when the screen is captured.

.. todo: :bolditalic:`resolution`

1. ``groups``  --  A JSON array of exactly 3 items, each of which
   corresponds to one of the groups in the component tree (red, green and blue).

   1. ``main``  --  The main component in a group.

      #. ``detection``  --  Information about the match method to use
         when trying to detect the region of interest
      #. ``interactions``  --  Details on any mouse or keyboard actions
         to initiate in the event of a positive detection
      #. ``visuals``  --  Information about the region of interest and
         the selection box laid over the screen capture

   2. ``subs``  --  A JSON array of exactly 4 items, each of which
      corresponds to one of the subcomponents in a group. The structure of
      each element is identical to the structure of the main component above.

2. ``screen``  --  The `Base64 <https://en.wikipedia.org/wiki/Base64>`_
   |alyvix-ext-link-icon| representation of the screen capture. There is
   exactly one screen capture for each resolution in a test case object.

.. todo: :bolditalic:`groups` ...

|


.. _alyvix_test_case_data_format_measure:

*Objects > {object} > Measure*
==============================

The *measure* structure holds some of the user-selected options
:ref:`visible in Selector <alyvix_selector_interface_headers>` for a given test
case object, along with the **series** array, which contains recorded data
resulting from each run of the test case object by Alyvix Robot in the test
case's script within a single session.

The measure structure is empty in a plain ``.alyvix`` file, but contains
measurements in a timestamped ``.alyvix`` file.

When a test case is executed, its ``.alyvix`` file is taken as the starting
point, and thus the **series** structure for all of its test case objects is
empty. Each time a test case object is called by the test case script, the
results and auxiliary data are added to **series** as a new array element.

.. todo: :file:`.alyvix`

This example shows the structure and some default values:

.. code-block:: json

   { "measure": {
       "group": "<transaction-group-name>",
       "output": "<true/false>",
       "series": [
           {
               "accuracy_ms": "<ms-amount>",
               "annotation": "<base64>",
               "exit": "<true/false/fail/not_executed>",
               "performance_ms": "<ms-amount>",
               "records": {
                   "check": "<true/false>",
                   "extract": "<extracted-text>",
                   "image": "<base64>",
                   "text": "<scraped-text>"
               },
               "resolution": {
                   "height": "<pixel-amount>",
                   "width": "<pixel-amount>"
               },
               "scaling_factor": "<zoom-amount>",
               "screenshot": "<base64>",
               "timestamp": "<epoch>"
           }
       ],
       "thresholds": {
           "critical_s": 5,
           "warning_s": 3 }
       }
   }

#. ``group``  --  The :ref:`transaction group <alyvix_selector_interface_headers>`
   assigned to the test case object

#. ``output``  --  Whether the test case object's "Measure" checkbox is marked

#. ``series``  --  An array containing the results and auxiliary data for
   each run of the parent test case object, containing the following information:

   #. ``accuracy_ms``  --  Given the performance measurement from the same
      run (see below), this is the interval (+/-) over which
      :ref:`that measurement <test_case_execution_measurements>` is guaranteed
      to have occurred

   #. ``annotation``  --  The `Base64 <https://en.wikipedia.org/wiki/Base64>`__
      |alyvix-ext-link-icon| screen captured when the test case object was
      being run, and overlaid graphic annotations to indicate those components
      that matched as well as potentially the first component that did not match

   #. ``exit``  --  The result type of the most recent run. If the test case
      object did not match, then the type ``false`` or ``fail`` is determined by
      whether the :ref:`break flag <alyvix_selector_interface_headers>` was set

   #. ``performance_ms``  --  The number of milliseconds that elapsed from
      when the run of a test case object began until a match was confirmed,
      subject to the accuracy interval described above; otherwise ``-1``

   #. ``records``  --  Temporary variables that are computed during the
      matching process, such as a text string extracted by a text component

   #. ``resolution``  --  The screen resolution at which the test case
      object was executed

   #. ``scaling_factor``  --  The Windows zoom level at the time when the
      test case object was executed

   #. ``screenshot``  --  The `Base64 <https://en.wikipedia.org/wiki/Base64>`__
      |alyvix-ext-link-icon| representation of the screen during the test case
      screen capture, but is made during the execution phase, rather than
      during the building phase

   #. ``timestamp``  --  The time (Unix epoch) when this series element
      was run; otherwise ``-1``

4. ``thresholds``  --  The number of elapsed seconds from when the test case
   object has been started until a ``warning`` and/or ``critical`` alert should
   be triggered; if no values are specified, no alerts will be triggered

.. todo: :bolditalic:`groups`

|


.. _alyvix_test_case_data_format_script:

*Script*
========

The **script** parameter records the
:ref:`main script and any subroutines <alyvix_editor_scripting_panel_top>`
used to compose more complex interaction behaviors.

.. code-block:: json

   { "case": [
        "<test-case-object-name>",
        { "disable": "<test-case-object-name>" },
        { "flow": "<test-case-object-name>",
          "if-true": "<test-case-object-name>" },
        { "flow": "<test-case-object-name>",
          "if-false": "<test-case-object-name>" },
        { "flow": "<test-case-object-name>",
          "for": "<test-case-map-name>" }
     ],
     "sections": {
        "<section-name>": [
           "<test-case-object-name>",
           "<test-case-object-name>"
        ],
        "exit": [],
        "fail": [] }
   }

#. ``case``  --  The main script that will be
   :ref:`executed by Alyvix Robot <alyvix_test_case_execution_top>`. The script
   is an ordered list of :ref:`scripting elements <alyvix_editor_scripting_node_expressions>`
   in which a simple, enabled element is represented just by the name of a test
   case object. Other elements have the following meanings:

   #. ``disable``  --  Skip this test case object and continue with the next
   #. ``flow``  --  Indicates the test case object or section that is the
      value of this key should be executed by Alyvix Robot in the context of a
      conditional (*if-true* or *if-false*) or a loop (*map*)
   #. ``if-true``  --  Evaluates whether the detection part of a given
      test case object would match, but without executing its actions
   #. ``if-false``  --  As above, but if the test case object would not match
   #. ``for``  --  Indicates there should be a loop over the values in the
      specified :ref:`Map <alyvix_editor_interface_maps>`

#. ``sections``  --  Corresponds to the
   :ref:`named subroutine scripts <alyvix_editor_script_mgmt_top>` created in
   Alyvix Editor. Each section is a parameter where its key is the name of the
   section and its value is the same as the ``case`` value described above.

   #. ``exit``  --  The (teardown) script to execute when a script has
      completed, regardless of whether it
      :ref:`succeeded or failed <alyvix_editor_interface_top_level_scripts>`
   #. ``fail``  --  The (teardown) script to execute when a script has failed
      during the execution of its test case objects

.. todo: :bolditalic:`case` ...

|
