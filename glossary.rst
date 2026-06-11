.. _glossary:

Glossary
========

A
--
.. glossary::
   :sorted:

   Agent
      Agent is a software that is to be installed on a node to monitor hosts and services, and
      serve as a means of collecting data for monitoring purposes.
      Agents, can be grouped into tenants, like Icinga Agent for Active monitoring purposes, Elastic Agent,
      Telegraf Agent, APM agent, AX Monitor Agent, etc.

   Availability
      *Availability* is a key performance indicator for Service Level Agreement (SLA) contracts. It
      is a metric that measures how much a given monitored object is working as expected and is
      calculated with a procedure that involves the total time during which the object was
      **actually** available and the time that it was contractually **expected** to be available. The
      latter is also called *Operational Time*; the procedure for computing availability is called
      *Availability Calculation*.

      Multiple factors influence both the **actual** time and **expected** time, which vary from
      contract to contract. See :ref:`Creating SLA contract <slm-create-sla-contract>` for more details.

      The states affecting *Availability* and as such count as *non-available* are **HARD DOWN** for Host Objects and
      **HARD UNKNOWN** as well as **HARD CRITICAL** for Service Objects.

   Availability Report
      An *Availability Report* contains a list of the hosts and services that are subject of an SLA
      contract together with their availability percentage (computed by the Availability Calculation)
      for each one.

   Average Availability
      The Average Availability measures the availability of host/services based on the average
      of all host/services.

B
--
.. glossary::
   :sorted:

   Beta Software
      A **Beta Software** is a |ne| component that is usually a
      single package that can be installed at will. A Beta Software is
      a part of a module, but in some cases it can also be a standalone
      software, that provides a specific functionality which is not
      yet part of NetEye. Multiple versions of a same Beta Software
      coexist in the repository, and, like for Preview Software,
      Beta Software is provided **AS IS**, with no guarantee of
      stability and maturity.

   Business Process
      A *Business Process* is a high-level logical service that is composed of multiple monitored
      objects (and potentially other smaller business processes) interrelated by logical operations.
      The state of this logical service is calculated by substituting the status of each individual
      monitored object into the business process' logical expression.

      By treating a logical service as if it were a monitored object, you can calculate its
      availability, create more complex check commands, and set up Grafana dashboards based on them.


C
--
.. glossary::
   :sorted:

   Calculation Period
      A *Calculation Period* is used in the **Service Level Management**
      module and is the unit of time over which the data will be aggregated into service
      level reports like an *Availability Report*.  For example, if the time
      span of the report is one year, you might want the Calculation Period
      to be a **Month** or a **Week**, depending on the required granularity.

   Client
      Client is a node that receives configurations from the Master directly
      or through a Satellite, implements them, runs checks, and reports
      back the results.
      Some clients, also known as agents, are to be configured manually
      in order to send data to a Satellite or to the Master, as in
      the case of a Telegraf agent.

   Corporate Network

      The network the NetEye Master or NetEye Satellite is connected to.
      In terms of networking involving a NetEye Cluster, a Corporate Network
      has to respect certain requirements for TCP and UDP ports that should be opened
      to allow NetEye to communicate correctly with the monitored hosts and external services.

      The ports can be inbound or outbound. Communication between the NetEye and Corporate Network
      should be built with respect to the NetEye architecture, which means selected ports
      are to be opened on the Master Node or its Satellite.

   Current |ne| Release
      The latest stable |ne| release available to be installed by a customer.
      Development cycle lasts two months, thus a new NetEye release is published and available for installation
      at the beginning of the even months.

      **Current** label in the NetEye User Guide indicates the version of the userguide which corresponds
      the current NetEye version.

      **Next** label corresponds to the next coming version of NetEye, which is currently in development.

      **Alpha** version appears in the User Guide for a two-week period, which means feature freeze of a to-be-released
      version and the start of a newer version development.

D
--
.. glossary::
   :sorted:

   Downtime
      In the context of monitoring within NetEye, *Downtime* is a scheduled period of time when a
      monitored object is intentionally either not available or will not perform its expected function,
      but which should nonetheless be considered as available.

      Downtime is typically planned announced in advance and is meant for periods of maintenance such as
      software or hardware upgrades.

      For further reference, please consult
      `Icinga's Downtime documentation <https://icinga.com/docs/icinga2/latest/doc/08-advanced-topics/#downtimes>`__.

E
--
.. glossary::
   :sorted:

   Event
      In the context of monitoring within NetEye, an *Event* refers to one of multiple possible event
      types, as declared by Icinga 2. The most common type is the
      `state change event <https://icinga.com/docs/icinga2/latest/doc/03-monitoring-basics/#hard-and-soft-states>`__
      caused by a host or service check result that differs from a previous check result.  An Event has
      a single timestamp, it is not a duration.

      The types of events currently defined on NetEye are:
      * **State Change:**  A host or service has changed from one state to another, e.g from OK to CRITICAL.
      * **Downtime:**  The host or service is scheduled to be down.
      * **Flapping:**  A host or service is continually alternating between two states, e.g. UP and DOWN.
      * **Comment:**  A NetEye user flagged a point in time with a written note.
      * **Notification:**  NetEye sends an alert, e.g. an email to a system administrator.

   Event Adjustment
      An *Event Adjustment* is a retroactive modification of the event history of a monitored object.
      The events' timeline and the actual event that take place **are not altered in any way**;
      rather, *Event Adjustment* find themselves on a separate layer on top of a timeline; They are
      manually applied in case check results were temporarily wrong (e.g., a faulty check command) or
      when an undesired outage happened due to incorrectly scheduled downtime.

      Event adjustments therefore do not directly alter the original event history: the
      original timeline, together with all events can always be reconstructed.

      Note that an Event Adjustment does not influence a Downtime: a monitored object in downtime
      during a given period is always considered as available, regardless of any Event Adjustment
      defined on the same period.

   External Event (Tornado)
      An input received from a datasource, whose format depends on its source.
      An External Event is arrives to a Collector, where it is converted into a Tornado Event,
      and then processed by a Tornado Pipeline.
      An example of input are events collected from rsyslog.

F
--
.. glossary::
   :sorted:

   Failover cluster
      A principle of organizing nodes in a cluster, which allows to
      avoid downtime or service disruption whenever one node in the
      cluster goes offline by means of moving services to another
      node.

   Federation
         User federation integrates external identity sources, like LDAP
         or Active Directory, with an access manager, enabling users to
         authenticate with existing credentials. It supports Single Sign-On
         (SSO) and centralizes identity management, improving security and
         user experience.

   Fencing
      In a cluster, *fencing* is the process of recovering services
      and resources running on a disconnected node by shutting it down
      and redistributing them on the other nodes. Fencing prevents data
      loss and maintains data integrity across the cluster.

   Final Sprint Release
      The final Sprint Release (SR) is the last Sprint Release of a given |ne| minor.
      It corresponds, in terms of content, to the |ne| Minor version released every two months.


H
--
.. glossary::
   :sorted:

   Host State
      The *Host State* is the reported state of a monitored host object in Icinga 2 at any point in time.

      As defined by Icinga, hosts can be in any one of the
      `following states <https://icinga.com/docs/icinga2/latest/doc/03-monitoring-basics/#host-states>`__:

      +------+---------------------------+
      | Name | Description               |
      +======+===========================+
      | UP   | The host is available     |
      +------+---------------------------+
      | DOWN | The host is not available |
      +------+---------------------------+

I
--
.. glossary::
   :sorted:

   Icinga DB Report
      A host or service availability report based on the monitoring database (Icinga DB).

   Identity Provider (IDP)
      An Identity Provider (IDP) is a service that authenticates users and provides identity information
      to service providers. It enables Single Sign-On (SSO) and federated identity management.

   Intracluster security
      It refers to the secure communication between the nodes in a
      cluster, granted by certificates signed by a Certificate
      Authority.


M
--
.. glossary::
   :sorted:

   Master
      The **Master** is a |ne| instance that holds configuration
      files, receives data directly from the clients or via
      Satellites and processes the data to carry out actions based on
      the customer business needs.

   Master Tenant
      The default Tenant in every |ne| installation. For single Tenant environments this is the only
      available Tenant.

   Monitored Object
      A *Monitored Object* is a host or service configured with a *check command* that can be checked
      either regularly and automatically (**active check**) or whenever requested (**passive check**).

   Monitoring Filter
      A *Monitoring Filter*, or *Filtering Expression* is a logical expression used to select a subset
      of monitored objects.

      For example, the following filter expression will select all hosts whose name begins with the
      string "server":

      .. code-block::

         host_name=server*

   Monitoring Status
      The monitoring status of the object, available in the Overview's monitoring panels,
      allowing you to check out the status or have a closer look at all the details of the check result.

   Multi-tenancy (or Multitenancy)
      A type of the system architecture that allows a single |ne|
      instance to monitor several business units in isolated
      environments.

N
--
.. glossary::
   :sorted:

   Natively Clustered Service
      (Also *Distributed replicated*) Services that use their own native clustering and inbuilt load balancing capabilities
      rather than Red Hat HA Clustering.
      Learn more about these services in :ref:`clustering-and-single-purpose-nodes`.


   |ne| Additional Feature Modules
      |neb| **Feature Modules** are |ne| components that perform very
      specific functions, and that can be installed on top of NetEye
      Core, thanks to its modular architecture.  Unlike :term:`Preview
      Software`, |ne| modules are officially supported; each module
      has its own, distinct contract, and can be quickly installed on
      demand. To learn how to do, simply check Section :ref:`How to
      install one additional module <neteye-install-modules>`.

   |ne| Components
      A |neb| **Component** is a software module that extends the
      functionalities of |ne| Core. There are three categories of
      |ne| Components: *|ne| Feature Modules*, *Preview Software*,
      and *Beta Software*. You can refer to Section
      :ref:`neteye-components` for detailed information.

   |ne| Core
      It is the set of most commonly used functionalities offered by
      NetEye, including monitoring, visualization (with dashboards and
      maps), configuration, reporting, and event handling.

   |ne| Cluster
      A setup type, where a system runs on a combination of various
      types of nodes: operative nodes, elastic-only nodes, and
      voting-only nodes; |ne| clustering service is based on a stack
      of software: Corosync, Pacemaker, and DRBD, although some
      NetEye services rely on their own clustering technologies.

   |ne| Cluster Node roles
      Some of the distributed NetEye services can be configured to run only on specific
      nodes within the Cluster. By modifying the Cluster configuration file
      one can assign specific services to specific nodes depending on the needs of the customer.

   |ne| Health Check

      It is important to monitor the health of NetEye itself. There are two types of |ne| health checks,
      that are each applicable for a number of reasons:
      * The Light check is a sequence of very lightweight checks that tells you quickly whether important parts of NetEye are up and running.
      * Deep checks are intended for tasks like verifying the integrity and consistency of resources. They're typically used before an update or upgrade.
      |ne| Health Checks are implemented as shell commands that call a set of scripts in a particular order.

   |ne| Services
      They are a selection of software, provided in packages, used to
      perform functionalities either within |ne| Core or |ne|
      Component(s).

   |ne| Single Node
      A |ne| setup type that is aimed at small environments that
      require limited resources.

O
--
.. glossary::
   :sorted:

   OAuth2.0
      OAuth 2.0 is an authorization framework that enables a third-party application to obtain
      limited access to an HTTP service, either on behalf of a resource owner or by allowing the
      third-party application to obtain access on its own behalf.

   OIDC
      OpenID Connect (OIDC) is an authentication protocol built on top of OAuth 2.0, providing
      user authentication and authorization. It enables Single Sign-On (SSO) and federated identity management.

   Operational Time
      The *Operational Time* corresponds to the expected time when determining the
      :term:`Availability` of a Monitored Object and is defined as the sum of all the Ranges during
      which a :term:`Monitored Object` must be properly working according to a SLA. While the
      Monitored Object is usually a single host or service, it can be a more complex object like a
      :term:`Business Process`: In this case, the *Operational Time* refers to all its components.

   Operative Node
      A |neb| **Operative Node** is the core node of a |ne| cluster.
      The Operative Nodes in a |ne| cluster are charged to run any
      local and shared service offered by |ne|, offering the High
      Availability of shared resources.

   Outage
      An *outage* is a period of time during which a Monitored Object or
      Business Process is **not available**, usually due to an unforeseen
      event. An outage starts when an object enters a state of
      non-availability and ends when it returns operative.

      To each outage corresponds a **Duration**, which is the total amount
      of time during which the Monitored Object or Business Process was not
      available.

   Outage Annotation
      An *Outage Annotation* is a user annotation for a specific outage, it is available in the
      SLA report. The annotation is associated with the outage by a date that falls within the outage period.


P
--
.. glossary::
   :sorted:

   PCS-managed Services
      In NetEye the high availability of some services is provided via PCS, while other services use their own clustering capabilities.
      Thus, *pcs-managed services* refer to one or more services that are started/stopped by PCS, which moves the service between nodes
      for high availability.

   Preview Feature
      A **Preview Feature** is a new functionality that has been
      developed and integrated into NetEye, but it is not yet in its
      final form.  A *Preview Feature* can be used **AS IS**, but not
      all functionalities are guaranteed to comply with the NetEye quality
      standards. Feedback on Preview Features is always appreciated!

   Preview Software
      A **Preview Software** is a new |ne| module that has been
      developed and integrated into NetEye, but it is not yet in its
      final form.  A *Preview Software* can be used **AS IS**, but not
      all functionalities are guaranteed to be stable and you should
      expect significant changes in the future.  Feedback on Preview
      Software is always appreciated!  Each module can be easily
      installed on demand with just a few commands: to learn how,
      check :ref:`How to install one additional module
      <neteye-install-modules>`.

   Private (Heartbeat) Network
      A type of networking that implies communication among the nodes composing the cluster,
      also “intra-cluster communication”. Intra-cluster communication should be usually freely allowed, however,
      Private (Heartbeat) Network should not be directly accessible from external networks.

R
--
.. glossary::
   :sorted:

   Range (Time Range)
      Within Icinga Director, a *Range* is the definition of a single
      unit of contiguous time; multiple ranges can be used as defining
      blocks of one :term:`Time Period`, for example `Time Period 24x5`
      can be defined as the union of `[TimeRange Monday 00:00-24:00,
      TimeRange Tuesday 00:00-24:00, TimeRange Wednesday 00:00-24:00,
      TimeRange Thursady 00:00-24:00, TimeRange Friday 00:00-24:00]`

      In the SLM API, Ranges are represented as a `ranges` object
      within a `time_period`.

   Realm
      A realm is an isolated environment for managing users, roles,
      and authentication, supporting multi-tenancy by segregating configurations
      and allowing independent management of security and identity settings per context.

   Realm role
      A realm role is a role assigned to a user or group within a realm,
      granting permissions to manage configurations in the authentication admin console.


   Repository Mirror
      Repository mirroring allows you to create and maintain a synchronized copy of
      a repository hosted outside your infrastructure. Using a mirror proves to be handy
      due to several reasons:

      *  A local synchronized mirror is closer and thus faster to reach
      *  You might want to replace the remote repository with your own internal one, which you have greater control over

   Resource Contract
      A **Resource Contract** is stipulated between an SLM customer and his service provider. The service
      provider uses NetEye to monitor the consumption of the resources and to report them in either pdf or
      html format to the SLM customer. Resources could be related to different monitoring objects, like
      CPU, RAM, Storage, or Network.

   Resource Report
      A type of an :ref:`SLM report <monitor-slm-resoure-reports>` based on a resource contract.

S
--
.. glossary::
   :sorted:

   SAML
      Security Assertion Markup Language (SAML) is an open standard for exchanging
      authentication and authorization data between an identity provider and a service provider.
      It enables Single Sign-On (SSO) and federated identity management.

   Satellite
      An intermediary node between monitored objects and the
      Master. A Satellite receives the configuration from its parent node
      (Master), collects data from hosts/services, and forwards the
      data to the Master to be processed. Satellite
      can also execute checks on its own, and pass the results to the
      Master afterwards.

   Service Level Agreement
      A *Service Level Agreement* is a contractual commitment between a service provider and a client
      defining particular quantitative aspects of a service.  It may specify the details of various
      metrics, thresholds, etc. such as:
      * Quality
      * Availability
      * Responsibilities

      In NetEye, particularly in the Service Level Management module, a single Service Level Agreement
      can be modeled as :ref:`an SLA contract <slm-create-sla-contract>`.

   Service Level Management
      *Service Level Management* is the practice (including methods and tools) of ensuring that monitored
      objects meet their target service levels.  It defines the basic units and metrics necessary for
      creating, measuring, accepting and documenting Service Level Agreements.

   Service Level Manager
      A *Service Level Manager*, as defined by ITIL, is a role that engages in the day-to-day management
      of a Service Level Agreement, with tasks that include documenting requirements, negotiating
      service levels and targets, conducting reviews, and ensuring to acted upon review results
      and provide them to customers.

   Service Owner
      A *Service Owner*, as defined by ITIL, is the role that is accountable for the delivery of a
      specific IT service.  The service owner is responsible for the service management of a specific
      service in the organization and typically controls funding for it and is also the
      representative and spokesperson of the service in the whole organization.

   Service State
      The *Service State* is the reported state of a monitored service object in Icinga 2 at any point
      in time.

      As defined by Icinga, services can be in any one of the
      `following states <https://icinga.com/docs/icinga2/latest/doc/03-monitoring-basics/#service-states>`__:

      +----------+----------------------------------------------------------------------------------------------+
      | Name     | Description                                                                                  |
      +==========+==============================================================================================+
      | OK       | The service is working properly                                                              |
      +----------+----------------------------------------------------------------------------------------------+
      | WARNING  | The service is experiencing some problems but is still considered to be in working condition |
      +----------+----------------------------------------------------------------------------------------------+
      | CRITICAL | The service is in a critical state                                                           |
      +----------+----------------------------------------------------------------------------------------------+
      | UNKNOWN  | The check could not determine the service’s state                                            |
      +----------+----------------------------------------------------------------------------------------------+

   Shutdown Command
      A **Shutdown Command** defines all the actions that have to be executed by the Shutdown Manager
      in order to power-down a host. Each Shutdown Command can contain variables that will be
      replaced on the *Shutdown host*.

   Shutdown Definition
      A **Shutdown Definition** is the specification describing groups of hosts that should be shut
      down when a specified *condition* on a host or a service is met, and the order in which those
      groups should be shut down.

   Shutdown Group
      A **Shutdown Group** contains a list of hosts which should all be shut down at the same time.
      Shutdown groups within the same **shutdown definition** can be given a relative ordering to
      determine which shutdown groups should be processed before another shutdown group.

   Shutdown Host
      A **Shutdown host** is a single host on which a *Shutdown Command* is executed. A Shutdown
      Host must have a *Shutdown Command* defined and can be part of one or more *Shutdown Groups*;
      if this is the case, the host will be shut down when the first group on which it is part of
      will be processed. In the subsequent groups, the host is simply ignored.

   Single Purpose Node
      A |neb| **Single Purpose Node** is a specialized Node in a
      |ne| Cluster. There are currently two types of Single Purpose
      Nodes: Elastic-only nodes, which are also marked ad **(E)**, and
      Voting-only nodes, which are also marked ad **(V)**. (E) are
      nodes that host the DB component of the Elastic stack. On the
      other hand, (V) are used by a |ne| Cluster as quorum devices.

   Sprint Release
      Sprint Releases (SRs) are specific versions of |ne| that are released at the end of a development Sprint.
      They represent a cumulative progression of the |ne| minor currently under development.

   State Change
      A *State Change* is one type of monitoring event where a host changes from one *Host State* to
      another (e.g., from UP to DOWN) or a service changes from one *Service State* to another (e.g.,
      from WARNING to OK).

T
--
.. glossary::
   :sorted:

   Target Availability
      *Target Availability* is the agreed-upon minimum time of guaranteed availability as specified
      contractually in a *Service Level Agreement*.  It is typically expressed as a percentage, such
      as 99.5%.

   Telegraf
      An agent to collect, process, and write metrics to InfluxDB.
      Telegraf agent sends data to a Satellite or to the Master.
      Telegraf instance is installed by default as part of |ne| core.
      A special *telegraf* package should be installed on external hosts.

   Template
      A generic set of properties of a monitored object, that will then be inherited from parent objects to their children.
      With the help of templates one can add and change configurations for hundreds of monitored objects with a single click.
      Based on the types of monitored objects, one can create a *Host template*, a *Command template* and
      a *Service template*.

   Tenant
      NetEye can be used to monitor objects belonging to multiple entities in such a way that
      each entity can independently collect and analyze only their own data. In this context,
      each entity is called a Tenant in NetEye.

      Agents can be grouped into Tenants. With respect to this setup type,
      NATS Server provides support for a secure, TLS-based, multi-tenancy, that can be secured using
      multiple NATS accounts, so that each Tenant's data flow is isolated.
      This grants self-contained, isolated communications from multiple clients to a single server.

   Time Frame
      Within the Icinga 2 Reporting module, a *Time Frame* specifies the starting and ending times
      for a given report and is a (positive) integer multiple of the **Calculation Period**.

      In the SLM API, Time Frames are represented as `time_range` objects consisting of `from` and
      `to` keys, with the `Unix Time <https://en.wikipedia.org/wiki/Unix_time>`_ as value.

   Time Period
      Within Icinga Director, a *Time Period* is a set of Ranges that
      together specify exactly when a monitored object should be
      available.  Its length is related to the *Calculation Period*
      and is used to specify the *Operational Time* in an :ref:`SLA
      Type <slm-create-sla-types>` for use in a :ref:`Report from SLM
      data <slm-create-report>`.

      In the SLM API, Time Periods are represented as `time_period`
      objects, while in the configuration files they are stored as
      **TimePeriod**\s. The alternative spelling of *Timeperiod* is
      also employed in the GUI.

      Icinga 2 provides documentation for the Timeperiod object as an
      `overview <https://icinga.com/docs/icinga2/latest/doc/08-advanced-topics/#time-periods>`__
      within the *Object Types* and in details under the
      `Advanced Topics <https://icinga.com/docs/icinga2/latest/doc/09-object-types/#timeperiod>`__.

   Tornado Collector
      A service which collects External events, converts them into Tornado events
      and forwards them to the Tornado Engine.
      Tornado :ref:`tornado-collectors-overview` run on the NetEye Master and on Satellites if there are any.
      There are different types of Collectors to be used depending on the type of event to handle.

   Tornado Processing Tree
      Processing Tree is composed of Filters, Iterators and Rulesets.
      One can modify Tornado configuration with the help of a Processing Tree.
      Tornado Engine receives and processes the events produced by the Collectors. The outcome of this
      step is fully defined by a Processing Tree, i.e. each Rule in a Ruleset determines:
      * The conditions a Tornado Event has to respect to match it
      * The actions to be executed in case of a match


U
--
.. glossary::
   :sorted:

   Unavailability Period
      An **Unavailability Period** is an interval of time during which a
      Monitored Object or a Business Process is a *not available* state due
      to an unforeseenable event, i.e., they should have been available but
      they were not, and no Downtime was scheduled.

      An Unavailability Period that occurs during an Operational Time Range
      is called an :term:`Outage`.

V
--
.. glossary::
   :sorted:

   Vulnerability
      NetEye considers a security vulnerability to be a weakness in our product or infrastructure
      that could allow an attacker to impact the confidentiality, integrity or availability
      of the product or infrastructure.

      For more details please check out NetEye's :ref:`bugfix-policy`.
