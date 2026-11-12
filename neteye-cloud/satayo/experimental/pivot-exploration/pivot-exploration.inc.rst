.. _satayo-pivot-exploration:

Pivot Exploration
~~~~~~~~~~~~~~~~~

.. warning::

   This is an :ref:`experimental feature <satayo-experimental-features>`: it
   can be changed or removed at any time, without prior notice.

**Pivot exploration** is a canvas where you chain steps to explore your
:ref:`findings <satayo_findings>`. The flow starts from a single finding: from
there you can load its related findings, filter them, branch on whether a
list is empty, and show the result.

Each step is a node on the canvas. You connect the nodes with links, and
every link carries a list of findings from one node to the next. For
example, you can start from a domain, load its related findings, keep only
the ones with a high priority, and open the result in the **Findings** page.

.. figure:: pivot-exploration/img/pivot-exploration.png

   A flow that starts from an IPv4 address and branches into filters, **If**
   nodes, and results

Enable the Feature
==================

Pivot exploration is hidden until you switch it on for a particular user:

#. Open the **Settings** menu in the header of the |sat| web interface.
#. Select **Experimental features**.
#. Turn on the toggle in the **Enabled** column of the **Pivot exploration**
   row.

The setting is saved in your user account: it follows you on every browser
and device, without affecting other users. To hide the feature again,
turn off the same toggle.

.. note::

   If the feature is off, links to a flow open the dashboard instead.

Open the Canvas
===============

Open the **Tools** menu in the header of the |sat| web interface and select
**Pivot exploration**.

Build a Flow
============

#. Every flow begins with a **Search** node. Type at least 3 characters to see a
   list of suggested findings, for example a domain, an IP address, an email
   address or a CVE, and pick one of them. Every step after the **Search** node works on
   that finding.
#. Click the **+** button (**Add next step**) on the side of a node and
   choose the next step. The new node is added and connected to that node.
#. To connect two existing nodes, drag a link from the output dot of one
   node to the input dot of another. Each input accepts only one link, and a
   link cannot create a loop.
#. To remove a node, click its **Delete node** button, or select it and
   press :kbd:`Delete` or :kbd:`Backspace`.

The flow runs again by itself every time you change it. Each node shows its
current status, such as the number of findings found, a loading message, or
the reason why it did not run:

* **An earlier step found no findings**: a node before this one returned an
  empty list.
* **This branch is not taken**: the node is on the branch of an **If has
  findings** node that the list did not follow.
* **An earlier step failed**: a node before this one could not load its
  findings.

Click **About this step** on a node to see what the node does and what it
is doing now.

Node Types
==========

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Node
     - What it does
   * - **Search**
     - The start of the flow. Pick one finding; it is the input for the rest
       of the flow. A flow has exactly one **Search** node.
   * - **Get related findings**
     - Loads the findings related to each finding on its input.
   * - **Base filter**
     - Keeps the findings whose column has one of the chosen values. Choose
       a column (*Type*, *State*, *Priority*, *Organization* or *Monitored
       domain*), then the values to keep. The values offered are the ones
       found in the incoming findings.
   * - **Advanced filter**
     - Keeps the findings that match a set of rules, joined with *and* /
       *or*, in the same way as the filters of the **Findings** page.
   * - **If has findings**
     - Shown as **If** on the canvas. It has two outputs: the list goes on
       through ✓ when it contains findings, and through ✗ when it is empty.
       Use it to run different steps depending on the result.
   * - **Type**
     - Keeps only the findings of one type. When a node shows its findings
       grouped by type, click a type to add a **Type** node for it.
   * - **Results**
     - Shows the findings on its link and has no output. Click **Expand
       details** to see the findings in a table, and **Open in findings
       page** to open them in the **Findings** page.

Limits Imposed for Efficiency
=============================

To keep flows responsive, some steps work on a limited number of findings:

* A flow has at most **50** steps.
* **Get related findings** follows at most the first **20** findings on its
  input, and loads at most **100** related findings for each of them. When
  some findings are left out, the node shows *The list is not complete*.
* A filter placed right after **Get related findings**, or after other
  filters that follow it, is applied while the related findings are loaded,
  so it works on all of them. In other cases, for example after an **If has
  findings** node, or when an **Advanced filter** that uses *or* follows
  another filter, a **Base filter** or **Advanced filter** takes at most
  **30** findings on its input, and only a complete list. Otherwise, the
  node asks you to narrow the list in an earlier step.
* An **Advanced filter** has at most **20** rules.
* **Open in findings page** works for at most **30** findings.

Save and Share a Flow
=====================

The flow on the canvas is saved automatically in your browser, separately
for each user account, so you find it again the next time you open the
canvas.

The flow is also stored in the address of the page: every change is a step
in your browser history, so the browser's **Back** and **Forward** buttons
undo and redo your changes, and you can bookmark a flow. When a flow
becomes too big for the address bar, |sat| shows a message: from then on,
**Back** and **Forward** no longer undo its changes, but the flow is still
saved in your browser.

To keep a flow or share it with a colleague, use the buttons in the top-right
corner of the canvas:

* **Export flow as JSON** downloads the flow as a JSON file.
* **Import flow from JSON** loads a flow from a file. After you confirm, the
  imported flow replaces the one on the canvas; this cannot be undone.

|sat| refuses a file that is not a valid flow file, for example a flow
with more than 50 steps or without exactly one **Search** node.

.. note::

   A flow refers to findings by their identifier, not by their content.
   Whoever opens a shared flow or link sees only the findings that their own
   account can access, and needs the feature enabled. If the starting
   finding cannot be loaded, the **Search** node asks you to search for
   another finding.
