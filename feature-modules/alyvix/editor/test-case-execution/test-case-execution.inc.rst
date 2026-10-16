
Alyvix test cases you've built with :ref:`Alyvix Editor <alyvix_editor_interface_top>`
can be run using Alyvix Robot. Robot can also execute any individual test
case objects you've created with Alyvix Designer, along with test
case objects you've duplicated with Alyvix Selector.

In a monitoring environment, the typical use case is to create a set of test
cases once, and then run those test cases repeatedly (ideally at a fixed
interval). Examples include monitoring the usability of proprietary
applications, streamed apps that inherently have no observability, and web
pages delivered from a remote server. A test case may measure responsiveness
every 5 minutes, for instance, and send timing and other data to a monitoring
system in the standardized output format it expects.

Before you can use Alyvix in a production environment, however, you'll first
need to iteratively develop and improve your test cases.

There are two principal modes for interacting with Alyvix Robot to ensure test
cases work properly during the development and testing phase, and that will
give you human-readable output.

* Calling Robot from :ref:`within Alyvix Editor <alyvix_editor_run_script>`
  via the ``Run`` button to run the script currently in Editor (note that in
  this mode you cannot export the resulting timing data)
* Calling Robot from the Command Prompt, passing it the name of a test case
  object, and using the default **-\ -mode** option with the ``alyvix``
  :ref:`option set <alyvix_robot_cli_options>`

.. todo: |runblue|

In either case, one test case is executed at a time.

.. note::

   You can also run one or more :ref:`individual test case objects <alyvix_editor_interface_debug>`
   of a test case's script which can be especially useful when debugging them.

|


.. _alyvix_robot_cli_launch:

Launching Alyvix Robot from the Command Prompt
``````````````````````````````````````````````

When Robot is run from the command prompt, it first checks to see if one or more
test case object names were passed via the **-o** parameter. If so, Robot will
execute those test case objects in sequence. Otherwise, if the **-o** parameter
is not specified, but the :file:`.alyvix` test case file named by the **-f** parameter
:ref:`contains a script previously defined in Editor <alyvix_test_case_data_format_script>`,
then Robot will run that script
(:ref:`see the -f option below for details <alyvix_robot_cli_options>`).

Multiple test case objects can be run in sequence by putting them in order
within quotation marks, as long as all those objects exist in the test case
(in this example, the objects "start" and "settings"):

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_robot -f start-test -o "start settings"

Adding the *verbosity* (**-v**) parameter will provide additional information
that can help you should you need to debug your test cases (see the section
:ref:`CLI Output Format <alyvix_robot_result_cli>` below).

Alyvix Robot will execute an Alyvix file's test case script when run with the
following command (with or without the :file:`.alyvix` suffix):

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_robot -f <alyvix-file-name>

If you used Alyvix Editor to create a file named :file:`start-test.alyvix`
containing a test case object named ``start``, you can run just that test case
object with this command:

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_robot -f start-test -o start

.. _alyvix_robot_cli_options:

Finally, the *args* (**-a**) parameter allows you to pass one or more values
(each treated as a string) to
:ref:`insert into the String field <alyvix_designer_options_strings_map_extract>`
of a component, and thus insert them into the user interface, for example in a
text field. If you need more than one argument, they should be enclosed in
double quotation marks. If an argument contains a space, that argument should
be enclosed with single quotation marks (in general, follow the rules of Python
for string management within the command line).

These options are summarized in the following table:

.. table::
   :widths: 25 10 15 50
   :width: 100%

   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | Option                     | Alias | Argument        | Description                                                |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -args                   | -a    | *<string(s)>*   | Supply one or more strings to use in the                   |
   |                            |       |                 | :ref:`String <alyvix_designer_options_strings_top>`        |
   |                            |       |                 | field when running a single test case object in Designer.  |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -filename               | -f    | *<name>*        | Supply the file name with or without extension.  Alyvix    |
   |                            |       |                 | assumes that if no path is included with the file name,    |
   |                            |       |                 | then the original test case is stored in the current       |
   |                            |       |                 | directory which Alyvix was launched from.  If you include  |
   |                            |       |                 | an absolute path with the file name, then it will look in  |
   |                            |       |                 | that path for the named file, and store the log file in    |
   |                            |       |                 | that same path, not in the current path.  You *should not* |
   |                            |       |                 | use relative paths.                                        |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -key                    | -k    | *<key>*         | Supply a private key for use with encryption.              |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -mode                   | -m    | *<name>*        | Specifies the desired output format.                       |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``alyvix`` -- [*default*] CLI output format for humans     |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``nagios`` -- Nagios output                                |
   |                            |       |                 | :ref:`(see below) <alyvix_robot_result_nagios>`            |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``nats-influxdb`` -- NATS to InfluxDB                      |
   |                            |       |                 | :ref:`(see below) <alyvix_robot_result_nats_influxdb>`     |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -object                 | -o    | *<name>*        | Supply the Object name(s).                                 |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -screenshot-saving      | -ss   | -\              | Save screenshot and annotation PNG files (or JPG           |
   |                            |       |                 | depending on the screenshot compression argument) in the   |
   |                            |       |                 | test case folder.                                          |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -screenshot-compression | -sc   | *<type*>        | Select the type of compression to use.                     |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``lossless`` -- [*default*] Records screenshots and        |
   |                            |       |                 | annotations in PNG format.                                 |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``compressed`` -- Records screenshots and annotations in   |
   |                            |       |                 | JPG 30% format.                                            |
   +----------------------------+-------+-----------------+------------------------------------------------------------+
   | -\ -verbose                | -v    | *<n>*           | Set the verbosity level for debugging output ranging       |
   |                            |       |                 | from **0** (min) to **1** (max).                           |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``0`` -- [*default*] Records start/stop timestamps, state  |
   |                            |       |                 | and time measures for each object (with measure option     |
   |                            |       |                 | enabled)                                                   |
   |                            |       |                 |                                                            |
   |                            |       |                 | ``1`` -- Also logs Alyvix actions                          |
   +----------------------------+-------+-----------------+------------------------------------------------------------+

|


.. _alyvix_robot_result:

What Alyvix Robot Returns
`````````````````````````

Alyvix uses the following industry-standard return values for monitoring systems:

.. table::
   :widths: 18 18 64
   :width: 100%

   +-------------+--------------+-------------------------------------------------------------------------------+
   | Error Level | Label        | Meaning                                                                       |
   +-------------+--------------+-------------------------------------------------------------------------------+
   | 0           | ``OK``       | The service responded and the results were within expectations                |
   +-------------+--------------+-------------------------------------------------------------------------------+
   | 1           | ``WARNING``  | Action should be taken to prevent a likely near-term problem from becoming    |
   |             |              | more serious                                                                  |
   +-------------+--------------+-------------------------------------------------------------------------------+
   | 2           | ``CRITICAL`` | A significant incident has already occurred and should be handled immediately |
   +-------------+--------------+-------------------------------------------------------------------------------+

With the **-\ -mode** option you can specify the format for the information
returned (defaults to CLI output mode).

|


.. _alyvix_robot_result_cli:

CLI Output Format
`````````````````

When run from the command prompt with the default **-\ -mode** parameter,
Alyvix Robot will both *(a)* display a short log describing basic events and
timing data, and *(b)* create a new file based on the original test case
:ref:`(see the -f option for details) <alyvix_robot_cli_options>`, but with the
detailed time measures added from that particular run. A timestamp
corresponding to the moment of execution will be appended to the file name:

:file:`<filename>_<full-timestamp>.alyvix`

For example:

:file:`start-test_20191220_145228_UTC+0100.alyvix`

When run from the command prompt with the default **-m alyvix** parameter,
Alyvix Robot will return human-readable output like the following when
successful:

.. todo: :iconlink:`video||videos_and_tutorials/operations_tutorials.html#operations-tutorials-outputcomposition`

.. todo: It would be better if the link pointed to our specific video tutorial rather than YouTube directly

.. code-block::
   :class: no-copybutton

   2019/12/12 18:24:20.405: start starts
   2019/12/12 18:24:21.949: open DETECTED in 0.0s (+/-0.260)
   2019/12/12 18:24:21.950: start ends OK, taking 1.544s.

If it fails instead, the output will appear like this:

.. code-block::
   :class: no-copybutton

   2019/12/12 18:37:41.448: start-test starts
   2019/12/12 18:37:51.762: settings FAILED after 10s
   2019/12/12 18:37:51.763: start-test ends FAILED because of settings, taking 10.315s.

If you've enabled the :ref:`break flag <alyvix_designer_options_test_case_object>`
in :ref:`Alyvix Selector <alyvix_selector_interface_top>` for a given test case
object, then if that test case object fails, no further test case objects will
be executed. If it's not set, then the test case object will instead be
skipped after its timeout has expired.

.. _alyvix_robot_result_cli_measures:

When you run Robot from the Windows command prompt, you can access the return
value programmatically as follows:

.. code-block:: doscon

   C:\Alyvix\testcases> echo %errorlevel%
   0


.. rubric:: Multiple Measures in CLI Output Mode

If a given test case object is run more than once in a single script,
regardless of whether it appears in a sequential node, a conditional or
a section, only the output of the object's last run will be displayed. However,
all results :ref:`are still stored <alyvix_test_case_data_format_measure>` on
the **series** element of the test case object. Should you need to see the
output of a single test case object when it runs more than once, best practice
is to use Selector :ref:`to duplicate it <alyvix_selector_interface_object_actions>`
(this will improve any data visualizations used with the data).

The exception is when a test case object appears in a loop. In this case, the
timing results will be output :ref:`for each key in the loop's map <alyvix_editor_interface_maps>`.
For instance, the following example might be output after running
:ref:`the script <alyvix_editor_scripting_panel_top>`
``FOR:my_map``\ ``Run:dataentry`` (note that individual map results appear in
the output with the pattern *<object_name>_<map_name>-<key_name>*):

.. todo: |for|:forblock:`my_map`\ |run|:runblock:`dataentry`

.. code-block::
   :class: no-copybutton

   2020/06/03 10:17:08.732: start-test starts
   2020/06/03 10:17:10.993: dataentry_loop-key1 DETECTED in 0.0s (+/-0.062)
   2020/06/03 10:17:13.370: dataentry_loop-key2 DETECTED in 0.0s (+/-0.060)
   2020/06/03 10:17:13.372: start-test ends OK, taking 4.640s.

|


.. _alyvix_robot_result_nagios:

Nagios Output Format
````````````````````

When Alyvix Robot is run from the command prompt, the **-\ -mode nagios**
command option will generate performance data in
`Nagios message format <http://nagios-plugins.org/doc/guidelines.html#PLUGOUTPUT>`__
|alyvix-ext-link-icon|. The main status result for the monitoring check will be
one of the following values:

.. table::
   :widths: 35 65
   :width: 100%

   +-----------------------------+----------------------------------------------------------------+
   | **test_case_output_status** | **Description**                                                |
   +-----------------------------+----------------------------------------------------------------+
   | ``OK``                      | When the test case exits with ``OK``                           |
   +-----------------------------+----------------------------------------------------------------+
   | ``WARNING``                 | When the test case exits with ``OK``, but the performance of   |
   |                             | at least 1 object is greater than its warning threshold and    |
   |                             | still lower than its critical threshold                        |
   +-----------------------------+----------------------------------------------------------------+
   | ``CRITICAL``                | When the test case exits with ``OK``, but the performance of   |
   |                             | at least 1 object is greater than its critical threshold       |
   +-----------------------------+----------------------------------------------------------------+
   | ``CRITICAL``                | Whenever the test case exits with ``FAILED``                   |
   +-----------------------------+----------------------------------------------------------------+

The time measurements for Nagios are specified as follows (note that *object_timeout_s* for
*SKIPPED* must be converted to milliseconds):

.. table::
   :widths: 22 40 38
   :width: 100%

   +-----------------------------+-----------------------------+----------------------------------------------------------+
   | **Test Case Object Status** | **Format**                  | **JSON Path**                                            |
   +-----------------------------+-----------------------------+----------------------------------------------------------+
   | ``DETECTED``                | <object_performance_ms>ms   | *object_name > measure > series[] > exit > true*         |
   +-----------------------------+-----------------------------+----------------------------------------------------------+
   | ``SKIPPED``                 | <object_timeout_s>ms        | *object_name > measure > series[] > exit > false*        |
   +-----------------------------+-----------------------------+----------------------------------------------------------+
   | ``FAILED``                  | ms |tab| :hint:`(no value)` | *object_name > measure > series[] > exit > fail*         |
   +-----------------------------+-----------------------------+----------------------------------------------------------+
   | ``NOT EXECUTED``            | ms |tab| :hint:`(no value)` | *object_name > measure > series[] > exit > not_executed* |
   +-----------------------------+-----------------------------+----------------------------------------------------------+

**First nagios output line:**

.. code-block::
   :class: no-copybutton

   <test_case_output_status>: <test_case_output_message> | duration=<test_case_duration_s>s;;; <object_01_name>=<object_01_performance_ms>ms;<object_01_warning_s>s;<object_01_critical_s>s;; <object_02_name>=<object_02_performance_ms>ms;<object_02_warning_s>s;<object_02_critical_s>s;;

**Second nagios output line:**

.. code-block::
   :class: no-copybutton

   FAILED transactions (from first to last): <failed_object_01_name>; <failed_object_02_name>

**Third nagios output line:**

.. code-block::
   :class: no-copybutton

   NOT EXECUTED transactions: <not_executed_object_01_name>; <not_executed_object_02_name>

|


.. _alyvix_robot_result_nats_influxdb:

NATS-InfluxDB Output Format
```````````````````````````

These parameters will publish all measures to an InfluxDB database through a
NATS channel. You will need the NATS server IP, port, subject name, and
measurement name.

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_robot -f vt -m "nats-influxdb <nats_streaming_server_ip>:<port> <influxdb_subject_name> <influxdb_measurement_name>"

|


.. _alyvix_robot_cipher_encryption:

Alyvix Cipher for Encryption
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use a combination of a cipher and a private key to protect sensitive
information and credentials such as user names and passwords when for instance
you need to log in to a protected application or website.
The ``Video Tutorials section`` provides
examples of how you can do this using Microsoft's RDC and Citrix,
while a :ref:`written tutorial <alyvix_example_test_case_authentication>` in the
Getting Started section shows an example web app login.

.. todo: :ref:`Video Tutorials <production_systems_tutorials_top>`

To encrypt a string, supply the text to be encrypted and your private key as
arguments to the *alyvix_cipher* command in a command prompt:

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_cipher -e <text_string_to_encrypt> -k <private_key>

You can also decrypt a cipher as follows, which returns the original string as
long as the private keys used in both cases are the same:

.. code-block:: doscon

   C:\Alyvix\testcases> alyvix_cipher -d <text_string_to_decrypt> -k <private_key>

Once you've computed the encrypted string, enter it into the
:ref:`string box <alyvix_designer_options_strings_top>` of an object component
(such as a text component indicating a user name or password field) and
save the test case.

There are two ways you can then use the encrypted string to hide credentials
until the moment they are entered into the interface:

* Using *alyvix_robot* from the command prompt as shown in the section above,
  add the string as the object of the *-\ -key* or *-k* parameter. This will
  decrypt the key for that single session.

  .. code-block:: doscon

     C:\Alyvix\testcases> alyvix_robot -f <test_case_name> -k <private_key>

* Using a scheduling system like *Alyvix Service*, add the private key in the
  appropriate place (e.g., in the
  :ref:`session management settings <alyvix_test_case_details>`).
  The credentials will then be passed automatically via the *-\ -key* parameter
  to Alyvix Robot each time it schedules a new run of the test case.

.. note::  With the current version of Alyvix Editor, you cannot launch Alyvix
           Robot while deciphering keys from within the Alyvix Editor interface
           :ref:`as described here <alyvix_editor_run_script>`. It will
           run the test case, but will insert the encrypted text without
           decrypting it.

|


.. _test_case_execution_measurements:


How Alyvix Measures Time
~~~~~~~~~~~~~~~~~~~~~~~~

Alyvix makes a series of screen captures multiple times a second in a process
called ``frame grabbing``. The system hardware determines exactly when a frame
is grabbed.

.. todo: videos_and_tutorials/operations_tutorials.html#operations-tutorials-measurement

At regular intervals, Alyvix takes one of those frames and tries to
``detect objects`` within it. The set of objects to look for is defined in the
test case object that is currently running in the script.

A ``transaction`` is the object detection phase plus the interaction phase, and
corresponds to the execution of the component tree including its actions.

``Transaction performance`` is the amount of time it takes to complete the
object detection phase. It's :ref:`the number that's displayed <alyvix_robot_result_cli>`
as the result in Alyvix's output when the Measure flag is set for that step.

Because frame grabs aren't continuous, there's a span of time between them
when you can't be sure exactly when an object appeared. The ``accuracy``
reported by Alyvix is the greater of the times before and after the
frames when the object is first detected.

The following animation shows the interrelationship between these concepts:

.. image:: images/measurement.gif
   :class: image-boxshadow
   :width: 99%
   :alt: Measurement animation

The object detector looks at the first in a set of frames to see if all the
objects in the component tree are present. If the entire component tree matches,
Alyvix will begin looking backwards through recent frames to find the first
frame that matched. The time between the last user interaction and the match
is the transaction performance.

When measuring transaction performance, Alyvix starts counting at the moment
the previous interaction sequence ends and the new object detection phase
begins. It stops counting as soon as it's able to match the current set of
objects it's looking for. At the end of each run, Alyvix then reports all of
the measured transactions for which the measure flag was set in the test case
object.

.. image:: images/transaction.png
   :class: image-boxshadow
   :width: 99%
   :alt: Transaction diagram

The amount of available hardware resources greatly affects how accurate Alyvix
can be. Because frame grabbing in the underlying hardware is performed by
Windows, you can add resources like RAM and CPU to increase the number of
frames grabbed per second, which will decrease the amount of time between each
frame and thus improve accuracy.
