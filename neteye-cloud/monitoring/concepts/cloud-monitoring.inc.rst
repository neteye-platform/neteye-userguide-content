Monitoring with |nec|
~~~~~~~~~~~~~~~~~~~~~

|nec| Monitoring is a fully-managed cloud monitoring solution.
Designed to provide continuous visibility and protection, the service is delivered
and operated entirely in the cloud, ensuring reliability, scalability, and reduced
operational overhead for the end-users.

Unlike traditional monitoring systems, |nec| Monitoring requires no complex setup or
day-to-day management on your side. The Network Operations Center (NOC) and Security Operations Center (SOC) teams
handle all aspects of monitoring, alerting, and response, so you can focus on your core business activities.

This user guide will help you understand how the service operates, how to interpret the reports
and notifications you receive, and how to collaborate effectively with the operations teams to maximize
the value of the monitoring solution.

.. rubric:: Areas Covered by Monitoring

- **Network Monitoring**: Monitors ports (dropped packets, errors), throughput (latency, traffic, bandwidth), and hardware devices.
- **System & Storage Monitoring**: Monitors hardware, operating systems, services (DNS, DHCP, Active Directory), and processes.
  Tracks system performance (CPU, RAM, disk) and stores relevant data.
- **Reporting**: Supports the creation and scheduling of reports, including predefined SLA parameters.
- **Event Management**: Collects events from various sources (SNMP traps, email, logs, SMS) and, based on logical conditions,
  triggers actions such as service status updates, notifications, or forwarding to the
  `Tornado event processor <https://neteye.guide/4.49/core-modules/tornado/concepts.html>`__.
- **Service Level Management**: Ensures IT service compliance with predefined SLAs using collected data. Includes SLA definition,
  monitoring, reporting, and optimization (available as an Evolutionary Activity - AEV).
- **Business Service Monitoring**: Maps relationships between IT components and aggregates them into business services.
  Monitors availability, serving as the basis for SLA monitoring (available as AEV).
- **IT Orchestration**: Enables Service Desk operators to securely execute administrative commands without special credentials,
  reducing risks of error (available as AEV).
