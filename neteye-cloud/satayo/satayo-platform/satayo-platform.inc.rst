.. _satayo-platform:

|sat| Platform
~~~~~~~~~~~~~~~

The header of the |sat| web interface is always visible, on every page.
It gives you the tools that work on the whole platform, not on one page only.
Use these tools to choose which data you see, and to find your way in |sat|.

.. figure:: satayo-platform/img/top-bar.png

   The header of the |sat| web interface

The right side of the header contains the following tools, from left to right:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Tool
     - Purpose
   * - :ref:`Time machine <satayo-time-machine>`
     - Shows your data as it was on a day in the past.
   * - :ref:`Organization selector <satayo-organization-selector>`
     - Shows the data of one organization, or of all your organizations.
   * - **Search**
     - Searches among all findings in |sat|. Type at least three characters.
       Press :kbd:`Ctrl` + :kbd:`Shift` + :kbd:`F` to open it from the keyboard,
       and :kbd:`Esc` to close it.
   * - **Help and guides**
     - Opens a link to this user guide, and shows the version of |sat|.
   * - **Settings**
     - Opens the :ref:`Settings <satayo-settings>` area. Only users with the
       **Tenant Admin** role see this icon.

.. _satayo-organization-selector:

Organization Selector
`````````````````````

All the data in |sat| belongs to an *organization*. Use the organization
selector in the header to choose which data you see. If you have access to more
than one organization, you can select:

- a single **organization**, when you want to focus on one monitored perimeter;
- :guilabel:`All organizations`, when you want an aggregated overview of all the
  organizations available to you.

To change the selection:

#. Select the organization selector in the header. It shows the name of the
   current selection, for example :guilabel:`All organizations`. A list opens.
#. Optionally, type a part of the name in the :guilabel:`Search` field to
   filter the list. Select the **x** icon in the field to clear the filter.
#. Select an organization, or :guilabel:`All organizations` at the top of the
   list.

The selection applies to all the pages that show data, such as the
:ref:`Dashboard <satayo-dashboard>` and the findings. It stays when you move
from one page to another. The selection is also part of the URL of the page, so
you can bookmark or share the URL to open the same view again.

.. note::

   When the :ref:`time machine <satayo-time-machine>` is active, the list shows
   the organizations that existed on the selected day.

.. _satayo-time-machine:

Time Machine
````````````

The **time machine** lets you travel back in time. |sat| keeps the history of
your data. When you select a day in the past, the pages show your attack surface
and your findings as |sat| saw them on that day.

Use the time machine to:

- check what your exposure looked like before a change, for example before an
  incident or before a remediation activity;
- compare the situation of today with the situation of a week or a month ago;
- find out when a finding appeared, or when it was closed.

The time machine does not change your data. You can use it as many times as
you want, and you can leave it at any time.

Open the Time Machine
'''''''''''''''''''''

Select the :guilabel:`Time machine` button in the header. A panel opens.
The panel contains:

- **Quick select**: the buttons :guilabel:`1 week`, :guilabel:`2 weeks`,
  :guilabel:`3 weeks` and :guilabel:`1 month` go back by that period from today.
- **Select a date**: a field in which you type a date (``dd/mm/yyyy``) or pick
  a day in a calendar.
- **Recent dates**: the days that you selected before. Select one to use it again.
  This list appears only after you have used the time machine.

Travel to a Day in the Past
'''''''''''''''''''''''''''

To go back by a fixed period, select one of the **Quick select** buttons.
|sat| applies the date at once.

To choose any other day:

#. Select the date field, or its calendar icon.
#. Select a day in the calendar. Use the arrows next to the month name to move
   to another month.
#. Select :guilabel:`Save`.

.. figure:: satayo-platform/img/time-machine-calendar.png

   Choosing a day in the calendar

You can select any day up to yesterday. There is no limit in the past, but
|sat| has data only from the time when it started to monitor your
organization. Today and the future are not available. To see the data of
today, :ref:`leave the time machine <satayo-time-machine-leave>`.

.. note::

   |sat| shows the data as it was at the **end** of the selected day
   (23:59:59), in the time zone of your browser.

Know That the Time Machine Is Active
''''''''''''''''''''''''''''''''''''

When the time machine is active, the header shows it in two ways:

- the button changes to :guilabel:`Time traveling to` and the selected date, and
  it has a glow;
- a notice :guilabel:`Time machine is active` appears next to it. Select the
  **x** icon to dismiss the notice. The button stays as it is, so you always see
  that you look at the past.

.. figure:: satayo-platform/img/time-machine-active.png

   The header when the time machine is active

.. warning::

   While the time machine is active, the numbers on the pages are **not** the
   current ones. Before you take a decision, check the date in the header.

Change the Day
''''''''''''''

Select the :guilabel:`Time traveling to` button, and select another quick
select button, or another day in the calendar and :guilabel:`Save`.
The tables go back to the first page when the date changes.

.. _satayo-time-machine-leave:

Leave the Time Machine
''''''''''''''''''''''

Select the :guilabel:`Time traveling to` button, and then select
:guilabel:`Clear`. The pages show the current data again.

.. figure:: satayo-platform/img/time-machine-save-clear.png

   The :guilabel:`Save` and :guilabel:`Clear` buttons

What Changes With the Time Machine
''''''''''''''''''''''''''''''''''

The date applies to all the pages that show your data. This includes the
:ref:`Dashboard <satayo-dashboard>` panels, the findings and their details, and
the tickets.

The following rules apply:

- **Organizations.** The :ref:`organization selector <satayo-organization-selector>`
  shows the organizations that existed on the selected day. If the organization that you selected did not
  exist yet, |sat| selects :guilabel:`All organizations`. A finding of an
  organization that did not exist yet is not shown.
- **Your access.** The time machine uses your **current** permissions. It never
  gives you access to more data than you have today. If an access was removed
  after the selected day, you do not see that data.
- **Settings.** The :ref:`Settings <satayo-settings>` pages always show the
  current configuration. The selected date stays active, and it applies again
  when you return to a page with data.
- **Reference data.** Some data has no history, for example the catalog of the
  CVEs. These pages show the current values.

.. tip::

   The date is part of the URL of the page. Copy the URL to give a colleague
   the same view of the same day. The date also stays when you move between
   pages and when you reload the page.
