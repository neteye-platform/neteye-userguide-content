
The Alyvix test cases list can be visualized by visiting the :menuselection:`Alyvix / Test Cases` page.
The table displayed in the page provides a user with an overview of the test cases scheduled on all Alyvix nodes,
useful to centralize, in a single view, the information coming from different nodes.

For each test case, the following data about its execution are displayed:

* **Name**: The name of the test cases
* **Node**: The name of the node on which the test case is scheduled
* **File**: The name of the file associated with the test case
* **Arguments**: The arguments used by the test case during its execution

For an easier lookup of the relevant information related to the test cases and their execution,
it is possible to search the table, by using the search bar, and sort the various columns,
clicking their name.

.. _figure-alyvix-testcases-page:

.. figure:: /feature-modules/alyvix/img/alyvix-testcases-page.png
   :alt: The Alyvix test cases page

   The Alyvix test cases page

Moreover, a dedicated option of the `More options` menu allows a user to delete, after confirmation,
a selected test case.

Furthermore, when visualizing the test cases, it is possible to add a test case by clicking
the `Create` button. A side panel will open with the possibility to schedule a test case on one
of the available nodes.

.. _alyvix-test-case-details:

Manage Test Cases details
`````````````````````````

You can view the details of a particular test case by clicking it in the test cases list. The details
panel for a selected test case will appear on the right, with details being grouped into Sessions and
General tabs.

.. _alyvix-test-case-sessions-tab:

Sessions Tab
++++++++++++

This tab reports all sessions a test case is scheduled for. Each session can be identified by its name and
the status of a test case in the session, either *enabled* or *disabled*. The status can be simply changed by
using the associated checkbox.

On the other hand, a session’s workflow and settings can be modified by clicking the arrow
button for a particular session item.

.. figure:: /feature-modules/alyvix/img/alyvix-testcase-details-sessions.png
   :alt: The Alyvix test cases sessions details

   The session details of a test case

.. _alyvix-test-case-general-tab:

General Tab
+++++++++++

General test case details include:

* **Name**: The name of a test case
* **Node**: The node a test case is scheduled for
* **File**: The file containing the test case definition, selectable among the files available on the node
* **Arguments**: The arguments used by a test case during its execution
* **Time Period**: Specifies during which time of the day and during which days the test case must be executed.
  You can select Time Periods that were previously configured as described :ref:`here <alyvix_create_a_time_period>`.
  By default the Test Case will be executed at all times.
* **Tags**: Used to categorize and differentiate the test cases in different entities that belong to the same
  tenant. They can be used to filter test cases for specific viewers depending on their role and so the tag can
  reflect the organization/company name, department or even the brand/product name.
  For example, imagine that in a Tenant **ACME** there is a test case **test_case_A** that is run for the department **depA**
  and another test case **test_case_B** for department **depB**. You want that users of department **depA** only see **test_case_A**,
  and similar for department **depB**. To achieve this, you can assign the tag ``depA`` to test case **test_case_A** and tag ``depB``
  to test case **test_case_B**, and then configure the role of department **depA** users to be restricted on tags ``depA``, as
  described in the :ref:`alyvix-tenant-viewer-role` section.
  You can associate multiple tags by selecting them in the dropdown list. If the tag you are looking for is not
  present, you can create a new one using the dedicated form by clicking the 'Add' button.

When visualizing the general details of a test case, it is possible to update the file,
arguments and the Time Period.

.. note:: In case the node has some time periods out of sync with respect to the corresponding definition in the
          Director, before updating a Test Case it will be necessary to sync the time periods definitions with the Alyvix Node, as
          mentioned in the :ref:`alyvix-timeperiods-tab` section.

.. figure:: /feature-modules/alyvix/img/alyvix-testcase-details-general.png
   :alt: The Alyvix test cases general details

   The general details of a test case

.. _alyvix-test-case-reports-tab:

Reports Tab
+++++++++++

A test case is executed periodically by Alyvix.
Each execution is called **test case run**.
The `Reports tab` is the place where the **runs** of a test case are shown.
Runs are ordered by date and time of execution, the most recent being added
at the top of the list.

Each run is described by the following data:

* **Result icon**: the result represented by a green icon for successfully run test cases, red icon for failed ones
* **Timestamp**: the date and time when the test case was run
* **Session**: the session in which the test was executed
* **Screenshot icon**: an icon is displayed if the run has an associated screenshot of the execution

You can filter test cases to view only failed runs with a toggle on top of the list.

Clicking on a specific run will open the Report view, which shows the following details:

* **Node name**: the name of the node where the test case was executed
* **Username**: the username of the RDP session used by the test case
* **Date**: the execution date
* **Time**: the execution time
* **Duration**: the time, in seconds, taken by the test case execution
* **Transactions list**: a list of the transactions composing the test case. For each transaction,
  its execution status and duration are reported.

By selecting a transaction, you can consult the transaction parameters and understand how it performed.
In particular, the following details are displayed:

* **Screenshots**: if available, the screenshot taken by the Alyvix Robot (possibly with annotations and the
  transaction definition).
* **Transaction parameters**: reporting the transaction performance and its specification.
