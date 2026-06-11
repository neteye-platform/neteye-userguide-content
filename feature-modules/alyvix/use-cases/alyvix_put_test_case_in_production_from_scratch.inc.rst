.. _alyvix-put-test-case-in-production:

Put Test Case in Production
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section explains how to put an Alyvix test case in production.

.. _prerequisites:

Prerequisites
`````````````

#. In order to be able to put a test case in production, the Alyvix Node
   should be installed as described in section :ref:`install-alyvix-node`, and
   then :ref:`configured <alyvix-create-an-alyvix-node>` in the Director Module.
   You should also follow the :ref:`alyvix-nodes-authentication` guide to
   configure secure communication with |ne|.

#. A new test case file should be created using `Alyvix built-in tools <https://alyvix.com/learn/test_case_building.html>`_.

#. To run a test case with an Alyvix Service, the Node must have a valid license.
   You can download the license request key and then upload the obtained
   license activation key directly from NetEye, as described in the :ref:`alyvix-license-tab` section.

.. note:: The interval between obtaining the license request key and activating the license
   in NetEye may take some time. Thus, make sure your license is activated before
   you start working with sessions and test cases.

After the Alyvix Node setup process including all the above has been completed, you can
proceed with creating your first Alyvix session.


Step 1. Create Alyvix Sessions
``````````````````````````````

A test case should be run within a session. To add it, switch to the
:ref:`nodes list <alyvix-nodes-list>`, select the Alyvix node and click the :guilabel:`New Session` button in the
:ref:`alyvix-sessions-tab`.

.. _alyvix_create_a_time_period:

Step 2. (Optional) Create a Time Period
```````````````````````````````````````

If you want to execute your Test Case every day, without any time limitations, you can safely skip this step.

If instead you want to run the Test Case only during a certain time of the day or on certain days of the
week (or even on certain dates), you should first create a **Time Period** which defines the exact hours
and days when the Test Case must be executed.

To create the Time Period navigate to :menuselection:`Icinga Director / Timeperiods / Timeperiods / Add` and define the
Name and Display Name of the Time Period. If you cannot access the Icinga Director module, please ask your NetEye administrator
to create such Time Period.

.. note::
   Please **do not** use the **Include period** and **Exclude period** functionalities
   because Alyvix does not support them and will not allow you to use such Time Period.

Once the Time Period is created, use the **Ranges** tab to define in which days and/or hours you want the Alyvix Test Case
to be run.

.. note::
   If you modify an existing Time Period which is already associated with an Alyvix Test Case, you should synchronize
   the changes with Alyvix in the Node's **Time Periods** tab. Click
   the :guilabel:`Sync with NetEye` button to sync all time periods definitions with the Alyvix Node, as described in the
   :ref:`alyvix-timeperiods-tab` section.

Step 3. Create Alyvix Test Cases
````````````````````````````````

Once you have added a session on the Alyvix node, switch to the :ref:`test cases list <alyvix-test-case-list>`
and click :guilabel:`Create`.

- Select the node to run a test case on
- Specify its definition by choosing the file previously created on the Alyvix Node as stated in :ref:`prerequisites`.

Step 4. Run Test Cases
``````````````````````

At this point, the test case you created is not running in its session. To enable its execution, click the
arrow button in correspondence with the session previously defined to open the session workflow controls.

- Enable the session if needed
- Enable the test case in the session using the associated toggle
- Start the session by enabling its workflow

Step 5. Check Test Case Results
```````````````````````````````

Once the test case has been executed, switch to the :ref:`alyvix-test-case-reports-tab` to visualize its runs.

- Click a specific run to open the Report view, which shows the details and all its transactions
- Select a transaction to consult its parameters and understand how it performed.
