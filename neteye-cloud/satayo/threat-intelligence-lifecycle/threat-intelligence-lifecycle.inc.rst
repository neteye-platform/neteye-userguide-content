

Threat Intelligence and Security Operations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Effective Security Operations rely on the ability to transform large volumes
of external information into timely, relevant, and actionable intelligence.
To achieve this, the company has built a consistent and repeatable process
that integrates Threat Intelligence into daily security operations.

This operational approach directly supports the capabilities described in
:ref:`satayo_service_overview` and complements the
:ref:`easm_with_satayo` model by ensuring that externally observed threats,
exposures, and adversary activity are continuously analyzed and translated
into consumable intelligence.


Threat Intelligence Lifecycle as a Foundation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Threat Intelligence Lifecycle defines a sequence of phases that guide the
production and operational use of intelligence. These phases are tightly
connected and form a continuous loop, ensuring that intelligence remains
aligned with evolving threats and organizational priorities.

The lifecycle phases adopted by the company include:

- Planning
- Collection
- Processing
- Analysis
- Dissemination
- Feedback

Each phase plays a critical role in ensuring that Threat Intelligence supports
decision-making and operational readiness.

.. rubric:: Planning Phase: Priority Intelligence Requirements

The journey begins with the **Planning** phase, during which
**Priority Intelligence Requirements (PIRs)** are defined.

A Priority Intelligence Requirement (PIR) is a specific, high-level question
or information need that guides the collection, analysis, and dissemination
activities of a Threat Intelligence program. PIRs represent the most important
intelligence priorities for the organization, directly supporting risk
management, operational decisions, and strategic awareness.

PIRs are not static. They are periodically reviewed and updated to reflect
changes in the threat landscape, business priorities, technologies, and
organizational perimeter.

.. rubric:: Collection Phase: Intelligence Sources

Once PIRs have been defined, the process continues with the **Collection**
phase. During this phase, information relevant to the identified intelligence
requirements is gathered from multiple sources.

These sources may include, but are not limited to:

- Open-source intelligence
- Technical telemetry
- Dark web and underground sources
- Human-driven intelligence gathering

The collected information constitutes the raw input used in subsequent phases
of the Threat Intelligence Lifecycle.

.. rubric:: Processing Phase: From Raw Data to Usable Information

The primary goal of the **Processing** phase is to transform raw data into a
usable, standardized, and accessible format suitable for analysis.

Raw data often arrives in heterogeneous formats and with varying levels of
reliability. Examples include logs, indicators of compromise (IoCs), malware
samples, dark web posts, and social media content. During the Processing phase,
this data is:

- **Cleaned**, by removing noise and duplicates
- **Normalized**, by converting it into a consistent schema
- **Enriched**, by correlating it with contextual information and metadata
- **Stored**, by indexing it and making it searchable for analysts and
  automated systems

This phase ensures that analysts and automated tools operate on consistent and
reliable datasets.

.. rubric:: Analysis Phase: Producing Intelligence

During the **Analysis** phase, processed information is evaluated and
contextualized to produce intelligence that directly addresses the defined
PIRs.

Analysis may be performed by human analysts or, for repetitive or
well-structured cases, by AI-based systems. The objective is to move beyond
raw data and produce insights that explain *what is happening, why it matters,
and what the potential impact is*.

Within its daily operations, the Threat Intelligence team produces multiple
types of intelligence, including:

- **Attack Surface Intelligence**
  Continuous discovery and monitoring of exposed assets such as domains, IP
  addresses, applications, and cloud services to identify vulnerabilities and
  potential entry points.

- **Technologies Intelligence**
  Analysis of technologies used across the organization or its sector to
  assess exposure, detect outdated systems, and anticipate threats targeting
  specific software stacks or services.

- **Dark Web Forum Intelligence**
  Monitoring underground forums and marketplaces to identify emerging threats,
  leaked data, or threat actor activity targeting the organization or its
  industry.

- **Brand Intelligence**
  Detection of brand misuse such as phishing domains, fake websites,
  fraudulent social media profiles, or logo abuse.

- **Supply Chain Intelligence**
  Assessment of cyber risks related to third-party vendors and partners that
  could indirectly impact the organization.

- **Threat Actor Intelligence**
  Profiling and tracking threat actors, including their motivations,
  capabilities, infrastructure, and tactics.

- **Vulnerabilities Intelligence**
  Identification and prioritization of vulnerabilities by correlating
  technical data with real-world exploitation trends.

- **Virtual HUMINT Intelligence**
  Human-driven intelligence collection in digital environments such as closed
  forums or chat groups to gain insight into threat actors’ intentions and
  plans.

.. rubric:: Dissemination Phase: Making Intelligence Actionable

After analysis, intelligence enters the **Dissemination** phase. The company
focuses on distributing intelligence in formats that are both consumable and
operationally actionable.

A significant portion of intelligence is disseminated through structured
tickets that provide detailed analysis and supporting evidence. These tickets
may include indicators of compromise or attack, such as IP addresses, domains,
URLs, hashes, hostnames, or email accounts.

Indicators are made available through multiple channels:

- In **raw format**, enabling direct consumption by security controls.
  For example, IP indicators can be ingested by firewall devices to
  automatically enforce blocking rules.

- In **enriched and correlated format**, through platforms such as MISP.
  This enables integration with SIEM systems and supports continuous threat
  hunting activities.

Concrete operational actions enabled by dissemination include automated
blocking, credential resets following data breaches, and continuous detection
of indicator-related events in monitored environments.

.. rubric:: Feedback Phase: Continuous Improvement

The **Feedback** phase is a critical but often overlooked part of the Threat
Intelligence Lifecycle. It ensures that intelligence production remains aligned
with real operational needs and delivers measurable value.

For the company, this phase includes structured, bimonthly meetings with
clients. During these sessions:

- PIRs are reviewed and refined
- The client’s threat landscape is discussed
- The monitored organizational perimeter is validated and updated

This is particularly important in environments where organizational boundaries
change frequently due to acquisitions or the adoption of new technologies.

.. rubric:: Supporting Integration Through RFI

To further support the integration of Threat Intelligence into Security
Operations, the company provides a **Request for Information (RFI)** module.

The RFI module enables different teams to submit structured analysis requests
on specific topics to the Threat Intelligence team. This mechanism promotes
cross-team collaboration, builds trust, and reinforces the role of Threat
Intelligence as a practical and valuable component of daily security
operations.
