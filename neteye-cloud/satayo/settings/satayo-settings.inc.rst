.. _satayo-settings:

Settings
~~~~~~~~

The **Settings** area shows how |sat| is configured for your tenant and all its organizations. To open
it, select the **Settings** (gear) icon in the header of the |sat| web
interface and choose one of the pages in the menu. The same pages are listed
in the side navigation of the Settings area.

.. note::

   The Settings area is available only to the **tenant administrator**, that
   is the user who can manage the roles of the tenant. Other users do not see
   the Settings icon, and if they open a Settings page through a direct link
   they are redirected to the Dashboard.

The following pages show the configuration of your tenant:

* :ref:`satayo-global-settings`: the intelligence requirements of your
  organization
* :ref:`satayo-keywords-settings`: the keywords monitored by each |sat|
  service

.. important::

   These pages are currently **read-only**: they show the configured values,
   but you cannot change them from the web interface. If a value is missing
   or must be updated, contact the Cyber Threat Intelligence team.

Page Layout
===========

The Global settings and Keywords pages share the same layout: a table with
one row per setting and the following columns.

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Column
     - Content
   * - Requirement / Service
     - The name of the setting, with a short description of its purpose.
   * - Status
     - Whether the setting is configured. A green tick means that the
       setting is configured, a warning icon means that it is only partially
       configured, and a red cross means that no data is available.
   * - Details
     - A summary of the configured values, such as counters and a preview of
       the first values. When there are more values than the preview shows,
       the number of the remaining ones is displayed as **(+N more)**.
   * - Action
     - When available, View All opens a window listing every
       configured value, while View opens the related findings.

If the settings cannot be loaded, an error message is shown together with a
Retry button.

