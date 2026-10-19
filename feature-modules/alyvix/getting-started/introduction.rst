
.. _alyvix_getting_started_top:


Tutorial Overview
~~~~~~~~~~~~~~~~~

Before building a test case in Alyvix, it's helpful to know what the available
options are and how to use them.  This section will walk you through a few simple
examples that demonstrate the basic functionality of Alyvix and how to use it
to build test cases.

To follow along, you'll need to open a Windows Command Prompt or PowerShell
session and switch to the directory where you want to keep your test cases,
for example:

.. code-block:: doscon
   :class: short-code-block

   C:\> cd C:\Alyvix\testcases

The examples below are designed to be explored in sequence, with the skills
building on each other, so that you can start from the simplest possible test
case and end with the ability to access websites with encrypted credentials
while measuring response time.

.. rubric:: 1. Open the :ref:`Windows Start menu <alyvix_example_test_case_windows_start>` with Alyvix Editor

* Build a simple test case that looks for and clicks on the Windows Start button
* Run the test case, so that Alyvix actually opens the Windows Start menu

.. rubric:: 2. Open the Start menu, then the :ref:`Settings app <alyvix_example_test_case_windows_settings>`
   immediately afterwards

* Build a second test case object, where the second depends on the results of the first
* Add the new step to the workflow from the previous example
* Execute the test cases, checking that they work together

.. rubric:: 3. Launch a web browser and :ref:`monitor a web page <alyvix_example_test_case_web_browser>`

* Launch an application by its executable path with command line parameters
* Measure how long it takes for the application to launch
* Kill the application to ensure there is only one instance the next time the test case runs
* Learn best practices when building test cases

.. rubric:: 4. Log in to a web app :ref:`using encrypted credentials <alyvix_example_test_case_authentication>`

* Use the credentials and a private key to create encrypted credentials
* Launch the web app as in the previous example and click on the login button
* Use the encrypted credentials in the login fields (Alyvix will automatically decrypt them)
* Do a simple task and then log out

|

.. todo:  You can find further examples, conceptual descriptions, and production-oriented use
          cases on the :ref:`Video Tutorials <video_tutorials_top>` page.

|
