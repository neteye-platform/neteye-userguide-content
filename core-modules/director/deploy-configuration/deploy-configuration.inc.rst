.. _deploy_config:

Deployment
~~~~~~~~~~

In Icinga2, configuration changes are stored in working memory until
they are *deployed*, i.e., the Icinga2 configuration is generated and
distributed to all agents in the zone. Initiating a **deploy** action
will check configuration validity, recording the details of the outcome.
There are two principal elements, both reachable from the main Director
menu:

-  **Deployment:** Manage the actions around deploying a new
   configuration.
-  **Activity Log:** Look at the deployment history, and carry out
   actions on that history.

.. _director_deploy_conf:

Deploying a Configuration
`````````````````````````

To deploy a modified configuration, go to **Director > Deployments**.
There you will see the **Deployments** tab (:numref:`figure-deployment-outcome`) that shows all
recent successful (green check) and failed (red ‘X’) deployment attempts
along with the date and time of those actions. Clicking on the link will
take you to the “Deployment details” panel that additionally contains
any warnings that may have been issued, and a ‘Configuration’ link that
will show you which configuration files were modified when the
deployment occurred.

.. _figure-deployment-outcome:

.. figure:: /core-modules/director/img/deployment05.png
   :alt: The deployments outcome panel

   The deployments outcome panel

Now click on the **Render config** action, which displays the
“Generated config” panel, a summary of the newly generated
configuration (:numref:`figure-deployment-outcome-panel`). This panel
contains a list of the files involved in the configuration changes,
including a summary of the number of objects per file by type
(objects, templates, or apply rules), and the new size of that file.

.. _figure-deployment-outcome-panel:

.. figure::  /core-modules/director/img/deployment08.png
   :alt: Deployment outcome panel

   Deployment outcome panel

You will now see three new actions:

* **Deploy pending changes:** Implement the deployment, which
  distributes the configuration to all Icinga2 agents. You can
  distribute the deployment even if there are no changes to make.
* **Last related activity:** Show differences between the current
  configuration and the most recent configuration before the current
  one.
* **Diff with other config:** Compare any two configurations using
  their unique identifiers (the numbers in parentheses in
  :numref:`figure-deployment-outcome`). The current configuration is
  inserted by default.

Activity Log
````````````

The Activity Log Panel (:numref:`figure-activity-log`) lets you look
at the history of successful deployments, and carry out actions on
items in that history.

The **My changes** action lets you switch from showing the history of
all changes to the view showing only those changes you made. You can
then click on **All changes** to return to viewing changes made by all
users.

.. _figure-activity-log:

.. figure::  /core-modules/director/img/deployment06.png
   :alt: The activity log panel

   The activity log panel

Each row represents a successful change to an object, coded for action
type, user (“**cli**” indicates an automated action) and time. The
action types are:

* Create (a blue “+” icon)
* Modify (a green “wrench” icon)
* Delete (a red “x” icon)

A duplicate timestamp over consecutive rows indicates those objects
were deployed at the same time. Clicking on the **modify** action type
in particular will take you to the **Diff** panel
(:numref:`figure-deployment-diff`) that will detail exactly what
changes were made.

.. _figure-deployment-diff:

.. figure::  /core-modules/director/img/deployment09.png
   :alt: Showing the differences between before and after configurations

   Showing the differences between before and after configurations

Once you have completed a successful deployment of monitoring objects
in Director, you can then go to the `host monitoring panel` (i.e.,g click on
a host under :menuselection:`Icinga Director --> Hosts --> Hosts`) to
check on the success of the overall monitoring configuration.
