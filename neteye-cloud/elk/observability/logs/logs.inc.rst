.. _elk-observability-logs:

Logs
~~~~

The **Logs** section is where you analyze the log data collected from your
applications, systems and infrastructure. It has three entries, each
suited to a different stage of an investigation:

* **Discover**: search, filter and read individual log entries when you
  know roughly what you're looking for.
* **Logs Anomalies**: let machine learning point you to the periods in
  which log activity deviated from its usual pattern, when you don't yet
  know where to look.
* **Logs Categories**: see which kinds of messages make up your logs and
  how often they occur, to spot recurring patterns in large volumes.

.. rubric:: Roles and Permissions

Depending on the role defined by the contract with your organization, you
might have different permissions:

* **VIEWER**: Read
* **EDITOR**: Read
* **ADMIN**: Read

Discover
````````

**Discover** opens the same application described in the
:ref:`Discover <elk-discover>` chapter of the Analytics section. Use it to
search and filter raw log entries and inspect their fields.

Logs Anomalies
``````````````

**Logs Anomalies** shows where the volume of log messages in a dataset
differs from what machine learning has learned to expect, for example a
sudden drop in the messages of an application or a burst of messages from
a host. These changes often indicate an incident, a regression or a
component that stopped working, and are hard to notice by reading logs.

Use the dataset filter and the time picker at the top to focus on the
datasets and period you're interested in. The chart shows one row per
dataset; each cell is colored by the anomaly score of that time interval,
from light blue (score below 3) to red (score 75 or higher), so you can
see at a glance when and where something unusual happened. The table below
lists the individual anomalies with their score, a short description (for
example *2x fewer log messages in this dataset than expected*), their
start time and dataset. Click a row to see the details and open the
affected logs.

.. figure:: /neteye-cloud/elk/observability/logs/img/logs-anomalies.png
   :alt: Logs Anomalies with the anomaly heat map per dataset and the table of detected anomalies
   :align: center

Start from the red and orange cells, or sort the table by anomaly score,
and check whether the period matches an alert, a deployment or a known
change. Anomalies are produced by machine learning jobs; click
:guilabel:`Manage ML jobs` to see which datasets are analyzed. For the
machine learning features in general, see :ref:`elk-machine-learning`.

Logs Categories
```````````````

**Logs Categories** groups log messages that have the same structure but
differ in details such as timestamps, IDs or values into categories. A
dataset with millions of lines typically has only a few hundred
categories, which makes it much easier to understand what the logs
contain.

For each category you can see an example message, how many messages
belong to it and how its frequency changes over time, and you can open
the matching log entries. Use it to find out which messages dominate a
dataset, to notice a category that appears for the first time or far
more often than before, and to filter out routine messages when looking
for the unusual ones.

.. seealso::

   * `Elastic guide to exploring logs
     <https://www.elastic.co/docs/solutions/observability/logs/explore-logs>`__
