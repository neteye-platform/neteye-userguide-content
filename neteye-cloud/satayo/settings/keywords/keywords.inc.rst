.. _satayo-keywords-settings:

Keywords
~~~~~~~~

|sat| uses keywords to detect data and information that are not directly
associated with your monitored assets, for example in forums, messaging
systems, social networks, and public source code. The **Keywords** page
lists the keywords configured for your tenant and all its organizations, with one row for each
monitoring service.

To open the page, select the **Settings** icon in the header of the |sat|
web interface and choose Keywords. The page is available only to
the tenant administrator and is read-only, as described in
:ref:`satayo-settings`.

For each service, the page shows the number of configured keywords and a
preview of the first five. When more keywords are configured,
View All opens a window with the complete list. A service
without keywords is shown with the **No data** status.

Services
========

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Service
     - Description
   * - General keywords
     - Keywords used to monitor your organization. They are required only for
       the main active domains. You can use the ``AND`` operator.
   * - SearchCode
     - Keywords searched in public source code.
   * - Twitter
     - Search rules for tweets. A space between two words means ``AND``.
   * - Keyword Phishing
     - Keywords used to look for :ref:`phishing domains <domain_phishing_item>`.
   * - Keyword Domain Link Logmonster
     - Keywords used by the domain link search in Logmonster.
   * - Darksearch
     - Keywords searched on Darksearch. You can use the ``AND`` operator.
   * - VirusTotal
     - Keywords used for the VirusTotal search.
   * - Keyword Market
     - Keywords searched in the markets. They are required for all active
       domains.
   * - Instagram
     - Keywords used for the Instagram search.

.. note::

   The Keywords row of the :ref:`satayo-global-settings` page
   counts only the general keywords.
