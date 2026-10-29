
..  _cra-incident-reporting:

Reporting a CRA Incident
------------------------

Use the reporting button immediately below to notify |witit| of an event that
falls under `Article 14 <https://eur-lex.europa.eu/eli/reg/2024/2847/oj/eng#d1e2337-1-1>`__
of the Act, that is:

 * A **vulnerability in** |neb| **that is being actively exploited**, or
 * A **severe security incident affecting the security of** |neb|

.. important::

   Report as soon as you become aware of the situation, without waiting for a complete
   analysis. Under Article 14 of Regulation (EU) 2024/2847, |witit| must submit an early
   warning to the competent CSIRT and to ENISA **within 24 hours** of becoming aware of an
   actively exploited vulnerability or a severe incident. Every hour of delay in reaching us
   reduces the time available to assess the situation and to warn other users.

   .. raw:: html

      <p style="text-align: right; margin-top: 1rem;"><a href="#" id="cra-report-trigger" class="cra-report-button">Report a CRA incident</a></p>
      <style>
        .atlwdg-trigger { display: none !important; }
        .cra-report-button {
          display: inline-block;
          padding: 0.6rem 1.2rem;
          background-color: var(--primary-color, #00A976);
          color: #ffffff !important;
          border: 1px solid transparent;
          border-radius: 2px;
          font-weight: 600;
          text-decoration: none !important;
          cursor: pointer;
        }
        .cra-report-button:hover,
        .cra-report-button:focus {
          background-color: var(--primary-color-light, #0BB798);
          color: #ffffff !important;
        }
        .cra-report-button:focus-visible {
          outline: 2px solid var(--cds-focus, #00A976);
          outline-offset: 2px;
        }
      </style>
      <script type="text/javascript" src="../_static/cra-report.js"></script>
      <script type="text/javascript" src="https://siwuerthphoenix.atlassian.net/s/d41d8cd98f00b204e9800998ecf8427e-T/-bxy8hv/b/1/c95134bc67d3a521bb3f4331beb9b804/_/download/batch/com.atlassian.jira.collector.plugin.jira-issue-collector-plugin:issuecollector/com.atlassian.jira.collector.plugin.jira-issue-collector-plugin:issuecollector.js?locale=en-US&amp;collectorId=850240eb"></script>

Please include, as far as it is known to you at the time of reporting:

    * The |ne| version and build in use, and the affected component or module
    * What you observed, and what leads you to believe the issue is being exploited
    * When the activity was first observed, and whether it is ongoing
    * The impact you have identified so far, and any systems known to be affected
    * Any indicators of compromise, log excerpts or artefacts you can share
    * A contact person we can reach for follow-up questions

Do not delay the report in order to complete this information. Send what you have and
follow up afterwards.

.. note:: This address is intended for actively exploited vulnerabilities and
   severe incidents only. For all other security findings, including
   vulnerabilities that are **not** known to be exploited, use the regular
   channels described in :ref:`Reporting Vulnerabilities <report-vulnerability>`.
   Reports sent to either address reach the same security team, so a report sent
   to the wrong address is never lost -- it may simply be handled with a
   different priority.

|


Manufacturer and Contact
------------------------

.. list-table::
   :width: 75%
   :widths: 30 70
   :header-rows: 0

   * - **Manufacturer:**
     - Würth IT Italy S.r.l.
   * - **Trade name:**
     - |ne|
   * - **Postal address:**
     - Via Johann Kravogl 4, 39100 Bolzano BZ, Italy
   * - **VAT ID:**
     - 01054690217
   * - **Email:**
     - info.italy@wuerth-it.com
   * - **Website:**
     - https://www.wuerth-it.it

To report vulnerabilities, see `Reporting Vulnerabilities <report-vulnerability>`.
For incidents and actively exploited vulnerabilities under Article 14 of the
Cyber Resilience Act, see Cyber Resilience Act Compliance.

|


Product Identification
----------------------

.. list-table::
   :width: 75%
   :widths: 30 70
   :header-rows: 0

   * - **Product name:**
     - |ne|
   * - **Product type**
     - Software product with digital elements (full managed and self managed
       monitoring platform)
   * - **Editions**
     - |ne|; |ne| with additional Feature Modules
   * - **Version scheme**
     - 4.x, released on a two-month cycle

|


What Happens After You Report
-----------------------------

    #. We acknowledge your report and assess whether it meets the criteria of Article 14.
    #. Where it does, we notify the competent CSIRT and ENISA within the statutory deadlines
       and keep you informed of the classification we applied.
    #. We inform affected users about the vulnerability or incident, about the corrective
       measures available and, where applicable, about mitigating measures they can take
       themselves.
    #. Once a fix is available, we publish a security advisory. Fixes follow the timeframes
       set out in the :ref:`Bugfix Policy <bugfix-policy>`.

Reporting an incident to us does not replace any reporting obligation you may have yourself,
for example under NIS2, DORA or data protection law.

.. Commented out for future work: the sections below are placeholders / pending
   CRA obligations. Re-enable once the content has been completed.

   When contacting us about a security matter, always state the full version and build
   identifier of the installation concerned.

   EU Declaration of Conformity
   ----------------------------

   .. todo:: To be added once the declaration has been signed. The obligation applies from
      11 December 2027. The document must be reachable from this page, and this page must be
      referenced from the user guide.

   CE Marking
   ----------

   .. todo:: To be added together with the EU Declaration of Conformity. For software, the CE
      marking is affixed either to the declaration or to this page.

   Security Support Period
   -----------------------

   .. todo:: State the support period and its end date for each release, and explain how the
      period was determined.

   Software Bill of Materials
   --------------------------

   The Software Bill of Materials for |ne| is made available to market surveillance authorities
   on reasoned request. Direct such requests to `security.neteye@wuerth-it.com
   <mailto:security.neteye@wuerth-it.com>`_.

|

Related Information
-------------------

    * :ref:`Reporting Vulnerabilities <report-vulnerability>` -- how to report a security
      finding that is not known to be exploited
    * :ref:`Definition of a Vulnerability <vulnerability-definition>` -- what we treat as a
      vulnerability
    * :ref:`Bugfix Policy <bugfix-policy>` -- resolution timeframes and severity levels
