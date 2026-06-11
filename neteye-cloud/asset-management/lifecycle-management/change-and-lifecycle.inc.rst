
Change & Lifecycle Management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A lifecycle is essentially a combination of a few core actions performed over
time, kept together on the asset record. It is a built-in functionality of GLPI,
which gives you full visibility and control over every stage of your IT assets’ lifecycle - from purchase to retirement.

Using Change & Lifecycle basics, you can achieve a number of important goals, such as:

* knowing the current state and history of any asset,
* quickly identifying assets due for maintenance, warranty renewal, or decommissioning,
* fast troubleshooting and decision-making by linking assets to users, locations, contracts.

Below you can find main functionalities of the GLPI's Asset Management Lifecycle.

Status as the Lifecycle Driver
``````````````````````````````

Each asset has a `Status <https://help.glpi-project.org/documentation/tabs/common_fields/status>`__ (labels depend on configuration, typically *In stock*,
*In use*, *Under maintenance*, *Retired/Disposed*). This is the simplest and most
important way to represent “where the asset is” in its lifecycle.

You can open the asset (for example under **Assets → Computers** / **Assets → Network equipment**)
and update **Status** in the asset form.

.. figure:: /neteye-cloud/asset-management/lifecycle-management/status.png

   Asset Status

Typical flow:

* **In stock** → available equipment (warehouse/spares)
* **In use** → deployed and assigned
* **Under maintenance** → temporarily unavailable (repair/diagnostics)
* **Retired/Disposed** → end-of-life, kept for history/audit


Assignment and placement
````````````````````````

As soon as an asset is deployed, you keep it aligned by linking it to:

* a **User** (and optionally a responsible **Group/Technician** in charge, depending on your process)
* a **Location** (physical site / office / rack / room)

This makes lifecycle information operational: status + user + location describe the real situation.


Procurement, stock, and delivery tracking
`````````````````````````````````````````

Lifecycle management also covers the “before production” steps:

* registering assets coming from procurement,
* keeping them in **stock/warehouse** until delivery,
* recording the **delivery/assignment** moment when the asset becomes “in use”.


Change documentation on the asset
`````````````````````````````````

During its life, an asset changes (upgrades, repairs, replacements,
configuration adjustments). In GLPI you capture this directly on the asset
record by:

* updating relevant fields (for example, key attributes that describe the item),
* adding **notes** for context (what changed and why),
* attaching **documents** (delivery notes, maintenance reports, manuals).


Financial, warranty, and contract context
`````````````````````````````````````````

To support lifecycle decisions (renew, repair, replace, retire), you keep:

* financial information (where applicable),
* warranty/coverage and/or links to :ref:`Contracts <contract_management>`.


Built-in history (traceability)
```````````````````````````````

GLPI keeps an automatic **history/audit trail** of updates on the asset, so you
can review how the asset evolved over time (status changes, assignment changes,
field updates).
