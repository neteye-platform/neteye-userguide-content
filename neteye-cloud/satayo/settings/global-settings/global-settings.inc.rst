.. _satayo-global-settings:

Global Settings
~~~~~~~~~~~~~~~

The **Global settings** page is available only to users with the
**Tenant Admin** role and is read-only, as described in
:ref:`satayo-settings`. It summarizes the intelligence requirements configured
for your organization. Clearly defined requirements, such as the Priority
Intelligence Requirements described in
:doc:`/neteye-cloud/satayo/threat-intelligence-lifecycle`, allow the analysis
team to focus on the resources that matter most to you and to respond faster.

To open the page, select the **Settings** icon in the header of the |sat|
web interface and choose **Global settings**.

Requirements
============

The page shows the following requirements.

.. list-table::
   :header-rows: 1
   :widths: 22 53 25

   * - Requirement
     - Description
     - Possible status
   * - Ransomware Monitor
     - The country, the sector, and the list of customers and suppliers used
       as criteria for monitoring
       :ref:`ransomware attacks <ransomware_item>`. Each of the three fields is
       shown with a tick or a cross.
     - Complete, Partial (for example 2/3), Missing
   * - Technology Mapping
     - The census of the relevant technologies used by your organization
       (vendors, products and versions), used for an efficient early warning
       process. **View All** lists every selected technology.
     - Mapped, Partial, Not mapped
   * - Resources
     - The subdomains identified and classified by severity: critical,
       potentially critical, risk accepted, and **Unassigned**. When potentially
       critical resources exist, an **Attention** message is shown.
       **View** opens the :ref:`DNS entry <dns_entry_item>` findings.
     - Configured, No data
   * - VIP Users
     - High-priority user accounts, such as executives and key personnel,
       that require enhanced monitoring and prioritized attention during the
       analysis.
     - Configured, No data
   * - Preferred Networks and IPs
     - The registered network ranges, grouped by netname (the network name
       field of the WHOIS record), and the preferred IP and CDN assets of your
       organization. **View All** lists every network with its subnets.
     - Detected, No data
   * - Mobile Apps (package names)
     - The :ref:`mobile application <mobile_app_item>` packages monitored
       across app stores.
     - Configured, No data
   * - Keywords
     - The number of general keywords configured and a preview of them. The
       keywords of the other monitoring services (such as SearchCode or
       Twitter) are listed in the :ref:`satayo-keywords-settings`
       page.
     - Configured, No data
   * - Git
     - The monitored :ref:`Git repositories <github_item>`.
     - Configured, No data
   * - Favicon
     - The :ref:`favicons <favicon_item>` configured for tracking and
       evidence matching.
     - Configured, No data
   * - Hostname naming convention
     - The convention used to name servers and workstations (for example
       ``srv-01`` or ``pc008382``). It helps to determine whether a
       compromised resource belongs to your organization or is personal.
     - Configured, No data
   * - Username naming convention
     - The convention used to name users (for example ``usr000382`` or
       ``admin_us_adams``). It helps to determine whether a user found in a
       data breach belongs to your organization.
     - Configured, No data
   * - Password policy
     - The password policy of your organization: minimum length, uppercase
       letters, numbers, special characters, and expiration. It helps to
       define the severity of the credentials found in a data breach.
     - Configured, No data
   * - BIN
     - The Bank Identification Numbers used to match
       :ref:`credit card <credit_card_item>` evidence. This row is shown only
       when at least one BIN is configured.
     - Configured
