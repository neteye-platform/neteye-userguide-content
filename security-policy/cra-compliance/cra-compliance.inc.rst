
..  _cra-incident-reporting:

Reporting a CRA Incident
------------------------

Use the address below to notify |witit| of an event that falls under Article 14 of the
Cyber Resilience Act, that is:

 * a **vulnerability in** |ne| **that is being actively exploited** —
   `report an actively exploited vulnerability
   <mailto:security.neteye@wuerth-it.com?subject=Vulnerability>`_, or
 * a **severe security incident affecting the security of** |ne| —
   `report a severe security incident
   <mailto:security.neteye@wuerth-it.com?subject=Security%20Incident>`_.

.. important:: **CRA incident contact:** `security.neteye@wuerth-it.com
   <mailto:security.neteye@wuerth-it.com>`_

   Report as soon as you become aware of the situation, without waiting for a complete
   analysis. Under Article 14 of Regulation (EU) 2024/2847, |witit| must submit an early
   warning to the competent CSIRT and to ENISA **within 24 hours** of becoming aware of an
   actively exploited vulnerability or a severe incident. Every hour of delay in reaching us
   reduces the time available to assess the situation and to warn other users.

Please include, as far as it is known to you at the time of reporting:

    * The |ne| version and build in use, and the affected component or module
    * What you observed, and what leads you to believe the issue is being exploited
    * When the activity was first observed, and whether it is ongoing
    * The impact you have identified so far, and any systems known to be affected
    * Any indicators of compromise, log excerpts or artefacts you can share
    * A contact person we can reach for follow-up questions

Do not delay the report in order to complete this information. Send what you have and
follow up afterwards.

.. note:: This address is intended for actively exploited vulnerabilities and severe
   incidents only. For all other security findings, including vulnerabilities that are not
   known to be exploited, use the regular channels described in
   :ref:`Reporting Vulnerabilities <report-vulnarability>`. Reports sent to either address
   reach the same security team, so a report sent to the wrong address is never lost — it
   may simply be handled with a different priority.

What happens after you report
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

   Manufacturer Information
   ------------------------

   .. todo:: Complete before publication. The legal entity, the postal address and a single
      verified contact address must be stated here. The user guide currently shows two different
      contact domains, and the footer names a different legal entity than the one used in the
      CRA documentation.

   **Manufacturer:** LEGAL-ENTITY-PLACEHOLDER

   **Trade name:** NetEye

   **Postal address:** POSTAL-ADDRESS-PLACEHOLDER

   **Contact:** CONTACT-EMAIL-PLACEHOLDER

   Product Identification
   ----------------------

   **Product name:** NetEye

   **Product type:** Software product with digital elements

   **Editions:** NetEye Core; NetEye Core with additional Feature Modules

   **Version scheme:** see :ref:`NetEye Releases <intro-neteye-releases>`

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

Related Information
-------------------

    * :ref:`Reporting Vulnerabilities <report-vulnarability>` — how to report a security
      finding that is not known to be exploited
    * :ref:`Definition of a Vulnerability <vulnarability-definition>` — what we treat as a
      vulnerability
    * :ref:`Bugfix Policy <bugfix-policy>` — resolution timeframes and severity levels
