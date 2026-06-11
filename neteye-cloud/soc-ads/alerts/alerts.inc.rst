.. _elastic-alerts:

Elastic Alerts
~~~~~~~~~~~~~~

This section will explore how Elastic Alerts work and explain how to analyze them.

There are five active :ref:`detection rules <soc-detection-rules>` within Elastic that generate alerts if anomalies or suspicious
behavior are detected. We'll see how to control these alerts here.

Overview
````````

You can access the **Alerts** section from the menu in the top left. It's located under the Security
category:

.. figure:: /neteye-cloud/soc-ads/img/security-alerts.jpg
   :align: center
   :figwidth: 50%

   The Alerts section within the Security category.

Once opened, the menu on the left will change and provide additional options:

.. figure:: /neteye-cloud/soc-ads/img/security-section.png
   :align: center
   :figwidth: 50%

   The Security section menu.

The interface contains a search bar and a time range at the top, three main sections called Open,
Acknowledged and Closed, a time graph, a summary table and one containing the details of the various alerts.

.. figure:: /neteye-cloud/soc-ads/img/alerts-interface.png


Details and management
``````````````````````

All triggered alerts have an *open* status and are displayed at the bottom along with the time
they were triggered, the rule that detected them, and other details. Each alert can be expanded
to view its entire contents, similar to how documents were **expanded** in Discover.

To analyze the event, what you usually do is **open** the Discover section in another window,
**set** the time range to when the alert was triggered and **filter** by host, user or event type (depending
on what you are looking for).

.. rubric:: I want to check the alert associated with the Spike rule in Logon Events

Steps:

 - Open a Discover in another tab, so as not to lose the details of the alert section.
 - Make sure you're using the correct index (`winlogbeat`).
 - Filter the period in which the alert occurred with the time range in the top right.

   .. note:: The time displayed next to the alert does not indicate the exact event itself,
      but rather when the rule reported it. It may therefore be necessary to analyze it
      a little earlier than that time.

 - Filter for login events by typing a query of your choice between the two below:
   - `event.action : logged-in`
   - `event.code : 4624`
 - Look for the peak reported by the alert and once found you can narrow the time range to exclude irrelevant data.

   .. figure:: /neteye-cloud/soc-ads/img/filter-by-timeperiod.png

      You can narrow the time period by dragging the cursor on the graph. A gray area will be highlighted, encompassing the results. You can also click directly on a peak.

 - Check the host values ​​in the documents to understand which machine is affected.

   .. figure:: /neteye-cloud/soc-ads/img/host-details.jpg

      You can click on one of the fields in the left column (not the + symbol) to get a percentage list of the values ​​present within the documents.

 - Finally, you can check the user who logged in, try to understand the reason, and, if it's due
   to a misconfiguration, try to fix it. You can also check if similar behavior has occurred on the same machine in the past (extending the time frame).

After reviewing an alert, I can change its status to *Acknowledged* or *Closed*.
This can be done by returning to the alert list and clicking on the three dots next to the alert in focus.

.. figure:: /neteye-cloud/soc-ads/img/alert-status.png
   :align: center
   :figwidth: 70%

.. note:: Some alert management options, such as closing, may not be immediately available due to a read-only user permission issue.
   These permissions will be updated to allow proper alert management.
