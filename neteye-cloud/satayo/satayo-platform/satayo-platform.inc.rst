.. _satayo-platform:

|sat| Platform
~~~~~~~~~~~~~~~

The header of the |sat| web interface is always visible, on every page.
It contains tools that work across the entire |sat|, not just on one page.
Use these tools to choose which data you see, and to find your way around |sat|.

.. figure:: satayo-platform/img/top-bar.png

   The header of the |sat| web interface

The right side of the header contains the following tools, from left to right:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Tool
     - Purpose
   * - :ref:`Time machine <satayo-time-machine>`
     - Shows your data as it was on a given day.
   * - :ref:`Organization selector <satayo-organization-selector>`
     - Shows the data of one organization, or of all your organizations.
   * - :ref:`Search <satayo-search-findings>`
     - Searches through all findings in |sat| (once you've typed at least
       three characters).
   * - **Help and guides**
     - Opens a link to this user guide, and displays the |sat| version.
   * - :ref:`Settings <satayo-open-settings>`
     - Opens the Settings area. Only users with the **Tenant Admin** role see
       this icon.

.. _satayo-organization-selector:

Choose Organization
```````````````````

All the data in |sat| belongs to an *organization*. Use the organization
selector in the header to choose which data you see. If you have access to more
than one organization, you can select:

- a single **organization**, when you want to focus on one monitored perimeter;
- :guilabel:`All organizations`, when you want an aggregated overview of all the
  organizations available to you.

To change the selection:

#. Choose the organization selector in the header. It shows the name of the
   current selection, for example :guilabel:`All organizations`, and opens a
   list.
#. Optionally, type a part of the name in the :guilabel:`Search` field to
   filter the list.
#. Select an organization, or :guilabel:`All organizations` at the top of the
   list.

The selection applies to all the pages that show data, such as the
:ref:`Dashboard <satayo-dashboard>` and the findings. It remains when you move
from one page to another. The selection is also part of the URL of the page, so
you can bookmark or share the URL to open the same view again.

.. note::

   When the :ref:`time machine <satayo-time-machine>` is active, the list shows
   the organizations that existed on the selected day.

.. _satayo-time-machine:

View Past Data
``````````````

The **time machine** lets you travel back in time, using the history of your
data kept by |sat|. When you select a day in the past, the pages show your
attack surface and your findings as |sat| saw them on that day.

Use the time machine to:

- check what your exposure looked like before a change, for example before an
  incident or before a remediation activity;
- compare the situation of today to what it was a week or a month ago;
- find out when a finding appeared, or when it was closed.

The time machine will not change your data. You can use it as many times as
you want, and you can leave it at any time.

Open the Time Machine
'''''''''''''''''''''

Select the :guilabel:`Time machine` button in the header, which will open a
panel containing:

- **Quick select**: the buttons :guilabel:`1 week`, :guilabel:`2 weeks`,
  :guilabel:`3 weeks` and :guilabel:`1 month` go back by that period from today.
- **Select a date**: a field in which you type a date (``dd/mm/yyyy``) or pick
  a day in a calendar.
- **Recent dates**: the days that you have used previously. Select one to use
  it again. This list appears only after you have used the time machine at
  least once.

Travel to a Day in the Past
'''''''''''''''''''''''''''

Select one of the **Quick select** buttons to go back by a fixed period. To
choose any other day, pick it in the date field and select :guilabel:`Save`.

.. figure:: satayo-platform/img/time-machine-calendar.png

   Choosing a day in the calendar

You can select any day up to yesterday. There is no limit for past dates, but
|sat| has data only from the time when it started to monitor your
organization. To see the data for today,
:ref:`leave the time machine <satayo-time-machine-leave>`.

.. note::

   |sat| shows the data as it was at the **end** of the selected day
   (23:59:59), in the time zone of your browser.

Know That the Time Machine Is Active
''''''''''''''''''''''''''''''''''''

When the time machine is active, the header shows it in two ways:

- the button changes to :guilabel:`Time traveling to` and the selected date, and
  it is highlighted with a glow;
- a notice :guilabel:`Time machine is active` appears next to it. Select the
  **x** icon to dismiss the notice. The button stays as it is, so you always see
  that you are looking at the past.

.. figure:: satayo-platform/img/time-machine-active.png

   The header when the time machine is active

.. warning::

   While the time machine is active, the numbers on the pages are **not** the
   current ones. Before you make a decision, check the date in the header.

Change the Day
''''''''''''''

Open the time machine again and choose another day. The tables go back to the
first page when the date changes.

.. _satayo-time-machine-leave:

Leave the Time Machine
''''''''''''''''''''''

Select the :guilabel:`Time traveling to` button, and then select
:guilabel:`Clear`. The pages show the current data again.

.. figure:: satayo-platform/img/time-machine-save-clear.png

   The :guilabel:`Save` and :guilabel:`Clear` buttons

What Changes with the Time Machine
''''''''''''''''''''''''''''''''''

The date applies to all the pages that show your data. This includes the
:ref:`Dashboard <satayo-dashboard>` panels, the findings and their details, and
the tickets.

The following rules apply:

- **Organizations.** The :ref:`organization selector <satayo-organization-selector>`
  shows the organizations that existed on the selected day. If the
  organization that you selected did not exist yet, |sat| automatically
  selects :guilabel:`All organizations`. A finding of an organization that did
  not exist yet will not be shown.
- **Your access.** The time machine uses your **current** permissions. It never
  gives you access to more data than you have today. If an access was removed
  after the selected day, you will not see that data.
- **Settings.** The :ref:`Settings <satayo-settings>` pages always show the
  current configuration. The selected date stays active, and it applies again
  when you return to a page with data.
- **Reference data.** Some data has no history, for example the catalog of the
  CVEs. These pages show the current values.

.. tip::

   The date is part of the URL of the page. Copy the URL to give a colleague
   the same view of the same day. The date also stays when you move between
   pages and when you reload the page.

.. _satayo-search-findings:

Search Findings
```````````````

Use the search in the header to find all the findings that match a value,
without opening the **Findings** page first. The search looks through all
findings in |sat|.

Select the **Search** icon in the header, or press :kbd:`Ctrl` + :kbd:`Shift` +
:kbd:`F`, and type at least three characters. The search shows a list of the
matching findings, with the matching value and the type of each finding.

Select :guilabel:`Show all` to see all the matching findings on the
**Findings** page, or select one finding to open it there with its details
already shown. Press :kbd:`Esc` to close the search.

.. _satayo-open-settings:

Open Settings
`````````````

Select the **Settings** (gear) icon in the header to open the Settings area,
which shows how |sat| is configured for your tenant and all its organizations.
Only users with the **Tenant Admin** role see this icon. For details, see
:ref:`satayo-settings`.
