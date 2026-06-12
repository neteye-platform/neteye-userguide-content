
.. _how_satayo_works:

How SATAYO works
~~~~~~~~~~~~~~~~

The SATAYO platform and its related services treat the continuous monitoring of
an organization's digital footprint. For better satisfying the needs,
the service was modelled using the Intelligence lifecycle as a reference model.

Starting from the assets provided during the onboarding phase, they could be considered
part of the information required, that represent the identity of the organization and its context;
the platform collects, processes, and analyzes data from multiple OSINT sources
to identify potential threats for the organisation.

The cycle is composed of multiple phases that are continuously repeated to ensure
that the intelligence remains up-to-date and relevant. It ends with a feedback stage
that allows refining the requirements and improve the overall process.

.. note:: It is essential to recognize that the process originates from the requirements.
   While the desired service outcome was defined during the onboarding phase, it is also
   necessary to establish the initial scope and identify the elements the service will be
   responsible for monitoring. For this reason, we specified a section in this guide dedicated to it.


After the definition of the requirement (scope of the service) and determination of the planning
of the activity, the intelligence lifecycle lays out a proper way to retrieve the data and normalize it.

SATAYO integrates a wide variety of different tools to perform the collection.
The search process is based on the asset domains that were previously identified, mapped,
and configured during the onboarding engagement with a SATAYO analyst.

The primary workflow used by the SATAYO engine consists of multiple scans performed
to assess the status of the monitored perimeter at a specific point in time. Each scan
leverages a dedicated probe to collect asset-related data. Starting from the domain level,
SATAYO independently reconstructs and continuously learns the infrastructure to be monitored.

Keywords are leveraged to detect data and information that are not directly associated with
the identified perimeter but are useful for deriving contextual insights and identifying elements
that represent the customer’s identity. A **typical use case** involves the application name of an Android app, which can be used
to identify additional applications maliciously crafted to impersonate the legitimate one.

Every **two months** a new exhaustive search starting from the domain assets is launched
and the result is stored and saved as a snapshot. This activity is a recursive process
that compares all new findings to the previous results, to show how the situation has changed.

Old searches remain available and can be checked in the History section.

During the two months some other granular scans are run daily or weekly and continue
to update the SATAYO Items. In the settings you can set up an e-mail or telegram notification
if you want to be notified of new research or discoveries.

The evidence collected by SATAYO are ordered in multiple :ref:`Items<satayo_items>` that can be reviewed.
A number called **Exposure Assessment Index Value (EAIV)** is calculated based on the evidence and highlights the :command:`Exposure Assessment` of the domain.
This value ranges from **0** to **100**, where zero means no exposure at all and 100 is the maximum value.
The higher the value, the higher the possible attack surface, with more information available online and potentially exploitable by threat actors.
Of course, as the company gets bigger, so will the score.

The items are divided into three major categories called **INFRASTRUCTURE**, **DATA FILES & PEOPLE** and **DEEP & DARK WEB**.
The Exposure Assessment is evaluated on them. You can download a :ref:`Global Report<global-report>` containing the data of all monitored domains
or a :ref:`Domain Report<domain-report>` for a single domain.

Scan times by type of Items
---------------------------

The following table lists the details of how often each scan is performed according to the type of item. All the evidence are described in details in the page :ref:`SATAYO Items<satayo_items>`.

+--------------------+-------------+
| ITEM               | PERIODICITY |
+====================+=============+
| HOSTNAME/IP        | 60 days     |
+--------------------+-------------+
| IP BLOCK           | 60 days     |
+--------------------+-------------+
| PORTS              | 60 days     |
+--------------------+-------------+
| DOMAIN SUSPICIOUS  | every day   |
+--------------------+-------------+
| DOMAIN CORRELATED  | 60 days     |
+--------------------+-------------+
| DOMAIN SIMILAR     | every day   |
+--------------------+-------------+
| DOMAIN TLD         | 60 days     |
+--------------------+-------------+
| DOMAIN PHISHING    | every day   |
+--------------------+-------------+
| FILE               | 60 days     |
+--------------------+-------------+
| VULNERABILITY      | every week  |
+--------------------+-------------+
| PHONE NUMBER       | 60 days     |
+--------------------+-------------+
| GENERAL SOCIAL     | 60 days     |
+--------------------+-------------+
| MAIL SERVER        | 60 days     |
+--------------------+-------------+
| BUCKET             | 15 days     |
+--------------------+-------------+
| MOBILE APPS        | 60 days     |
+--------------------+-------------+
| EMAIL              | every day   |
+--------------------+-------------+
| SOCIAL & SERVICES  | 15 days     |
+--------------------+-------------+
| BREACHED ACCOUNTS  | every day   |
+--------------------+-------------+
| PASSWORD           | every day   |
+--------------------+-------------+
| OPEN BUG BOUNTY    | every week  |
+--------------------+-------------+
| TELEGRAM           | every day   |
+--------------------+-------------+
| X                  | every day   |
+--------------------+-------------+
| DARK WEB FORUMS    | every day   |
+--------------------+-------------+
| DATA LEAK SITES    | every day   |
+--------------------+-------------+
| LOG STEALER MARKET | every day   |
+--------------------+-------------+
| SANDBOXES          | every day   |
+--------------------+-------------+
| CREDIT CARD        | every day   |
+--------------------+-------------+

|
|
