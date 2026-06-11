.. _tornado-gui:

Processing Tree
~~~~~~~~~~~~~~~

The **Processing Tree** presents all filters and rulesets within a Tenant.
The order of rules within a ruleset defines the sequence of their execution.

In order to continuously improve UX and usability of the Tornado Instance,
NetEye provides a GUI based on the `Carbon Design System's <https://www.carbondesignsystem.com/>`_ best practices.


.. _fig-tc-processing-tree:

.. figure:: img/carbon/processing_tree.png

   Example Processing Tree.


Use drag & drop function focusing on a button to the left of the rule name to change the order.

For the start, select the tenant that you would like to work with in the **toolbar**.
You can find more information on the relation between Tornado configuration and your tenants
in :ref:`tenant-based-tornado-configuration`.

More information on :ref:`tornado-processing-tree-conf` can be found in
a dedicated section.

``Edit`` Mode
`````````````

Switch to **Edit** mode in order to modify Tornado configuration
with the help of the Processing Tree.

When you start modifying the configuration, Tornado will continue to work
with the existing configuration-thanks to the implicit lock mode, while
the new changes are saved in a separate *draft configuration*.
The new configuration then must be deployed to become operative.

Edit mode has other positive side effects: one does not need to
complete the changes in one session, but can stop and then continue
at a later point; another user can pick up the draft and complete
it; in case of a disaster (like e.g., the abrupt end of the HTTPS
connection to the GUI) it is possible to resume the draft from the
point where it was left.

.. _enable-edit-mode:

.. rubric:: Enable 'Edit' Mode

Before you start modifying Tornado configuration with the help
of a Processing Tree, make sure that editing permission is granted
by the NetEye role.

#. In your NetEye installation, go to (:menuselection:`Configuration / Access Control / Roles`)
#. Select an existing role or add a new one for configuring the permission
#. Set the *tornado*/edit permission on the Tornado *Module* section of the role details to 'On'

.. figure:: img/carbon/edit-permission.png

    Tornado Module permissions

As a result, 'Edit' switcher would be available in the top right corner
of the Tornado Configuration.


Implicit Lock Mode
``````````````````

Only one user at a time can modify the Processing Tree configuration.
This prevents multiple users from changing the configuration simultaneously,
which might lead to undesirable results and possibly to Tornado not working
correctly due to incomplete or wrong configuration. When a user is editing
the configuration, the actual, running configuration is left untouched: it continues
to be operative and accepts incoming data to be processed.

.. warning:: Only one draft at a time is allowed; that is, editing
   of multiple draft is **not** supported!
