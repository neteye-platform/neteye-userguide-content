

.. _alyvix_designer_options_strings_top:

The String Field
````````````````

When a component in a test case object matches an area currently onscreen, its *Action* will
be executed, and then the contents of its :guilabel:`String` field will be typed out as a
sequence of keystrokes, one at a time, to the window in focus when the test case object matched.

.. image:: images/ad_action_string_sized.png
   :class: image-boxshadow zoomable-image
   :width: 80%
   :alt: The mouse action selection dropdown.

The text that is inserted may come from more than one source, not just what is typed in by hand in
the :guilabel:`String` field, in which case that field specifies how they should be combined
(examples can be found in the next section below):

* **Manually specified on the test case object itself:**

  * :bolditalic:`Regular characters:`  Normal letters and numbers, along with most punctuation
  * :bolditalic:`Special characters:`  Non-printable characters (see table below), such as
    :kbd:`Enter` and :kbd:`Control`, along with punctuation that has special meaning such as
    for regular expressions
  * :bolditalic:`System keys:`  Special key combinations (see table below) for the operating
    system, such as :kbd:`Win` + :kbd:`E` to open a new Explorer window.

* **Text read dynamically from the screen (scraped)**, computed by one test case object and
  then inserted as specified by another test case object that comes later in the execution
  of the test case script:

  * :bolditalic:`Full text:`  The source is the entire text found in a region of interest
    specified by the earlier test case object
  * :bolditalic:`Mapping extracted text:`  Given a extract text from a region of interest,
    pass it to the map, and insert the map's output value as specified by a later test case
    object

* **External sources:**

  * :bolditalic:`Map values:`  The source of the text is fields in the Map itself
  * :bolditalic:`CLI arguments:`  Text derived from command line parameters passed to `Alyvix Robot <https://alyvix.com/learn/test_case_execution.html#alyvix-robot-cli-options>`__

The extraction and mapping functions have a common purpose:  to substitute text from a source
string into a template string, and then send the resulting string to the application.

If the text is extracted directly from the screen, you will typically need to have two separate
test case objects:  one which acquires the text from the screen, and another containing the
template which inserts the text into the GUI object (the second object must occur chronologically
after the first, or else the text string will not yet have been scraped).  If the text comes
from one of the two external sources (map or arguments), only a single test caseobject is necessary.



.. _alyvix_designer_options_strings_functions:

Escaped String Examples
+++++++++++++++++++++++

To indicate that content in the String field template is not regular text, it must be escaped
with a pair of curly braces ``{ ... }``.  If you want to insert more than one template, each one
must go in its own separate set of curly braces.

.. note::

   For the purposes of scraping and mapping text, matching is applied in a case insensitive
   fashion.  However, the result is stored (and later retrieved) with the case of the original
   characters.

The following examples provide an illustration of how these string functions can be applied.

|



.. rubric:: Mixing Regular and Special Characters


Regular letters and numbers, along with most punctuation and special characters can be inserted
in the :guilabel:`String` field normally.  However, some special characters, editing keys, and
key combinations must be escaped.  The :ref:`table below <alyvix_designer_options_strings_special>`
lists the full set of special keys that can be used.

In the following example, a test case object with an Excel\ |trade| spreadsheet as a target would
put the words "First", "Second" and "Third" in three adjacent columns.

.. code-block::
   :class: tiny-code-block nocopy

   First{tab}Second{tab}Third

|



.. rubric:: Full Text

When a text component matches an area onscreen, all of the text in its region of interest is
scraped and stored.  This text can then be used by the test case objects in later scripting nodes
to insert strings into GUI fields.  The entire text can be inserted using the syntax
``{<test-case-object-name>.text}``.  So if a test case object named ``temperature_read`` reads
the string ``37 degrees`` in the application, it can be copied to another GUI field by putting
the following expression in the :guilabel:`String` field of a later scripting node:

.. code-block::
   :class: tiny-code-block nocopy

   {temperature_read.text}

|



.. _alyvix_designer_options_strings_map_extract:

.. rubric:: Mapping Extracted Text

Given a map with pairs of inputs to match and their corresponding output values, you can match text
scraped from a previous test case and insert the map's output value for that text.  On the first
test case object's component (which must be of type text), the type must be set to :guilabel:`MAP`
and the :ref:`map's name <alyvix_editor_interface_maps>` must be chosen in the :guilabel:`Map`
dropdown.  In the :guilabel:`String` field on the later test case object, the map's output
value can be retrieved with the template ``{<test-case-object-name>.extract}``.

For instance, if the string ``Paris`` from the test case object ``city`` is read from the screen,
and the selected map contains the pair ``Paris, France`` then the following template will generate
the text ``Country:  France``.

.. code-block::
   :class: tiny-code-block nocopy

   Country:  {city.extract}

|



.. rubric:: Map values

This option allows you to create a map with a table of values (where each row in the table
corresponds to one iteration of the loop).  For instance, you could insert three values per row
into a table on a web page or in a spreadsheet with this :guilabel:`String` field entry:

.. code-block::
   :class: tiny-code-block nocopy

   {1}{tab}{2}{tab}{3}

|



.. _alyvix_designer_options_strings_map_cli:

.. rubric:: CLI Arguments via the Alyvix Robot *-\ -args* parameter

Text can also be inserted from a command line parameter passed to Alyvix Robot.  To use this
capability, `pass Alyvix Robot the argument <https://alyvix.com/learn/test_case_execution.html#alyvix-robot-cli-launch>`__ as a simple keyword
after the ``-a`` parameter, and type the ``{<number>}`` notation into the :guilabel:`String` field
of the test case object that should insert the text.  For instance in the string below, passing the
parameter ``-a Red`` will result in the text ``Color: Red`` appearing in the target text field of
the application.

.. code-block::
   :class: tiny-code-block nocopy

   Color: {1}

|



.. rubric:: {<n>} Key Precedence and Defaults

You may have noticed that both :bolditalic:`map loops` and :bolditalic:`CLI arguments` share the
``{<n>}`` notation.  In fact, this notation is just a shortcut, which can be very useful since
it's rare to have to use both at the same time.  Still, should you need to insert an indexed key
when both a loop is running and you passed arguments via the command line, then the ``{<n>}``
notation will return the `map value <https://alyvix.com/learn/test_case_data_format.html#test-case-data-format-description>`__ before it would
return the value from the CLI argument list.  To ensure you are inserting the string from the
source you intended, you can use the following expanded notation:

+---------+-------------------------+----------------------+----------------------+
| Source  | Format                  | First Argument: {1}  | Second Argument: {2} |
+---------+-------------------------+----------------------+----------------------+
| CLI     | {cli.arg<n>}            | {cli.arg1}           | {cli.arg2}           |
+---------+-------------------------+----------------------+----------------------+
| Map     | {<map-name>.<key-name>} | {map1.key1}          | {map1.key2}          |
+---------+-------------------------+----------------------+----------------------+

Both the map loops and CLI arguments also use the same default notation:

.. code-block::
   :class: tiny-code-block nocopy

   {<key>,<default-value>}

So, for instance, you can use the following to insert the first CLI argument if it exists (and
you are not in a map loop), but insert the string "abc" if no arguments were passed to Alyvix
Robot:

.. code-block::
   :class: tiny-code-block nocopy

   {1,abc}



.. _alyvix_designer_options_strings_special:

Special Characters
++++++++++++++++++

The following table details which keys and characters require escaping when used.  Although our
standard is to always use lower case, the key sequences are case insensitive.

Note that this list does not include mouse events like
:ref:`scroll actions <alyvix_designer_options_components_common>` for browsers.

+-----------------------------------+-----------------------------------------------------------------+
| Key Sequence                      | Result                                                          |
+-----------------------------------+-----------------------------------------------------------------+
| ``{enter}``                       | **Enter** key                                                   |
+-----------------------------------+-----------------------------------------------------------------+
| ``{space}``                       | **Space** key (this is only needed at the beginning or the      |
|                                   | end of a string)                                                |
+-----------------------------------+-----------------------------------------------------------------+
| ``{tab}``                         | **Tab** key                                                     |
+-----------------------------------+-----------------------------------------------------------------+
| ``{shift}``                       | Press and immediately release the **Shift** key, without        |
|                                   | applying it to the letters that come next.                      |
+-----------------------------------+-----------------------------------------------------------------+
| ``{shift down}``, ``{shift up}``  | ``{shift down}`` holds the Shift key down until                 |
|                                   | ``{shift up}`` is encountered, so that all letters between      |
|                                   | them are shifted to upper case.   ``{lshift}`` specifies the    |
|                                   | left Shift key and ``{rshift}`` specifies the right one.        |
+-----------------------------------+-----------------------------------------------------------------+
| ``{ctrl}``                        | Press and immediately release the **Control** key, without      |
|                                   | applying it to the characters that come next.                   |
+-----------------------------------+-----------------------------------------------------------------+
| ``{ctrl down}``, ``{ctrl up}``    | ``{ctrl down}`` holds the Control key down until                |
|                                   | ``{ctrl up}`` is encountered, so that all characters in         |
|                                   | between have the Control function applied. ``{lctrl}``          |
|                                   | specifies the left Control key, and ``{rctrl}`` the right.      |
+-----------------------------------+-----------------------------------------------------------------+
| ``{alt}``                         | Press and immediately release the **Alt** key, without          |
|                                   | applying it to the characters that come next.                   |
+-----------------------------------+-----------------------------------------------------------------+
| ``{alt down}``, ``{alt up}``      | ``{alt down}`` holds the Alt key down until ``{alt up}``        |
|                                   | is encountered, so that all characters in between have the      |
|                                   | Control function applied. ``{lctrl}`` specifies the left        |
|                                   | Control key, and ``{rctrl}`` specifies the right one.           |
+-----------------------------------+-----------------------------------------------------------------+
| ``{lwin down}``, ``{lwin up}``    | ``{lwin down}`` holds the left **WindowsLogo** key down         |
|                                   | until ``{lwin up}`` is encountered                              |
+-----------------------------------+-----------------------------------------------------------------+
| ``{backspace}``                   | **Backspace** key                                               |
+-----------------------------------+-----------------------------------------------------------------+
| ``{del}``                         | **Delete** key                                                  |
+-----------------------------------+-----------------------------------------------------------------+
| ``{esc}``                         | **Escape** key                                                  |
+-----------------------------------+-----------------------------------------------------------------+
| ``{f1}`` - ``{f24}``              | **Function** keys                                               |
+-----------------------------------+-----------------------------------------------------------------+
| ``{numpad0}`` - ``{numpad9}``     | **Number pad** keys                                             |
+-----------------------------------+-----------------------------------------------------------------+
| ``{up}``                          | **Up arrow** (cursor) key                                       |
+-----------------------------------+-----------------------------------------------------------------+
| ``{down}``                        | **Down arrow** key                                              |
+-----------------------------------+-----------------------------------------------------------------+
| ``{left}``                        | **Left arrow** key                                              |
+-----------------------------------+-----------------------------------------------------------------+
| ``{right}``                       | **Right arrow** key                                             |
+-----------------------------------+-----------------------------------------------------------------+
| ``{home}``                        | **Home** key                                                    |
+-----------------------------------+-----------------------------------------------------------------+
| ``{end}``                         | **End** key                                                     |
+-----------------------------------+-----------------------------------------------------------------+
| ``{pgup}``                        | **Page Up** key                                                 |
+-----------------------------------+-----------------------------------------------------------------+
| ``{pgdn}``                        | **Page Down** key                                               |
+-----------------------------------+-----------------------------------------------------------------+
| ``{u+nnnn}``                      | A `unicode link <http://www.unicode.org/charts/>`_,             |
|                                   | where ``nnnn`` is its hexadecimal value, excluding the ``0x``   |
|                                   | prefix                                                          |
+-----------------------------------+-----------------------------------------------------------------+
| ``{{}``                           | **{**                                                           |
+-----------------------------------+-----------------------------------------------------------------+
| ``{}}``                           | **}**                                                           |
+-----------------------------------+-----------------------------------------------------------------+

|

All other printable keys not found in the above list can be used normally.
