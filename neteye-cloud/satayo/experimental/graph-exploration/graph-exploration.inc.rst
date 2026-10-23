.. _satayo-graph-exploration:

Graph Exploration
~~~~~~~~~~~~~~~~~

.. warning::

   This is an :ref:`experimental feature <satayo-experimental-features>`: it
   can be changed or removed at any time, without prior notice.

**Graph Exploration** shows your :ref:`findings <satayo_findings>` as an
interactive graph, so that you can see how they connect to each other. Two
findings are connected when |sat| marks them as related, or when they share
a value, such as a domain, an IP address or an email address.

Starting from a single finding, you can expand the graph step by step and
discover, for example, which other findings point to the same IP address, or
which exposed email addresses belong to the same domain.

Enable the Feature
==================

Graph Exploration is hidden until you switch it on for a particular user:

#. Open the **Settings** menu in the header of the |sat| web interface.
#. Select **Experimental features**.
#. Turn on the toggle in the **Enabled** column of the **Graph exploration**
   row.

The feature is available immediately, without reloading the page. The
setting is saved in your user account: it follows you on every browser and
device, and it does not affect other users. To hide the feature again, turn
off the same toggle.

.. note::

   If the feature is off, links to a graph open the **Findings** page
   instead.

Open the Graph
==============

#. Go to the **Findings** page and click a finding to open its preview.
#. Click the **Show in graph view** icon button in the header of the
   preview.

The graph opens with the selected finding as its first node, and its preview
is shown beside the graph.

From the graph you can:

* Click **Show in table view** in the header of a finding preview to open
  that finding in the **Findings** page.
* Click **Back to findings** to go back to the **Findings** page.
* Click **Reset graph** to start again from the first finding.

Reading the Graph
=================

The graph contains two kinds of nodes, connected by two kinds of lines.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Element
     - Meaning
   * - **Circle**
     - A finding. Its color shows the priority of its ticket: *Highest or
       High*, *Medium*, *Low or Lowest*, or *No ticket priority*.
   * - **Rounded square**
     - A value that two or more findings share. It can be a domain, an IPv4
       address, an email address, a CVE, a phone number, a favicon hash, a
       username, an author, a domain registrant, a Google Analytics ID or a
       certificate organization.
   * - **Dashed line**
     - A related finding, as stored by |sat|.
   * - **Solid line**
     - Goes from a finding to a value that the finding contains, or to the
       finding that represents that value, for example an *Email Address*
       finding.

In addition, a node can be marked in one of the following ways:

* **A ring around the node**: the finding is the one currently displayed in
  the preview.
* **A faded node**: its connections are still loading.
* **(+N) after the node's label**: the node has N more connected findings
  than the ones shown in the graph.

To display the legend of shapes, colors, lines and markers, click the **i**
button in the bottom-left corner of the graph.

Explore the Graph
=================

* **Click a finding** (circle) to open its preview and display its
  connections.
* **Click a value** (rounded square) to display the findings that contain
  that value.
* **Click Close** under a node to hide the connections that the node opened.
* **Select an organization** in the organization selector in the header to
  hide the findings of the other organizations.

Each expansion of the graph is a step in your browser history: you can use
the browser's **Back** and **Forward** buttons to move through your
exploration. You can also copy the address of the page and share it with a
colleague who has the feature enabled; the graph is rebuilt from the data
available when the link is opened.
