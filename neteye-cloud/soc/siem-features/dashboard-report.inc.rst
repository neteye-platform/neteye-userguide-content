
Dashboard and Report
~~~~~~~~~~~~~~~~~~~~

This section describes the data visualization and reporting functionalities of NetEye SIEM.

The customer's designated employees will have access to the platform via an authenticated
https web interface, with read-only permissions. The platform can be accessed using
a common browser and there are no limitations in the number of users that can simultaneously
access the tool.

Access will be given to several dashboards, which will contain the information of interest
monitored by the Cyber Security Analysts.

The dashboards make you aware of the normal behavior of users, applications and devices
and allow you to detect any anomalies that may indicate threats to the organization’s cyber
security.

Dashboards allow you to drill down into the relevant section to the desired level in order to
gain access to detailed information and be able to investigate certain logs. A number of
default dashboards have been developed to cater for different visualization needs,
and the Cyber Security Analysts team is available to customize dashboards for specific customer
requirements.

From the various configured dashboards, reports in pdf format can be generated and shared.

The following is an example of a view of the dashboards that are made available to the customer by default:

.. figure:: /neteye-cloud/soc/img/default-dashboard.png

   Default Dashboard view.

The list of reports and their periodicity of generation is given below:

.. csv-table::
   :header: "Report title", "Description", "Periodicity"

   "SOC report", "In this report you will find all information about the status of your company
   over the past month. Included are all the operations performed by our SOC, such as alerts and
   tickets handled. In addition, there will be a focus on cybercriminals with a list of ransomware
   attacks carried out this month, a list of new zero-day vulnerabilities, general vulnerability
   trends, security bulletins and much more.", "Monthly"
   "SOC ADS report", "This report contains all information on the activity of system administrators.
   The number of logins, logouts and failed logins is monitored, as well as the machines
   used by the administrators. Please note that for a more complete and in-depth view, Elastic
   is always available.", "Monthly"
   "Red Team report", "In this report you will find information on the attack simulation that is carried out at the end of the onboarding phase.", "Una tantum"
   "VA report", "In this report you will find all the information about the status of your company over the last month, regarding any vulnerability identified on the public perimeter that has been shared.", "Monthly"
   "Weekly report - Malicious IP", "This report contains information and statistics on the connection
   between internal IP addresses (subnets 10.0.0/8 or 172.16.0/12 or 192.168.0/16) and external IPs on one or more blacklists.
   Communications prevented, dropped, denied and rejected by the firewall are excluded.", "Weekly"
   "Informative report", "This report contains information and statistics on the connection between internal IP addresses (subnets 10.0.0/8 or 172.16.0/12 or 192.168.0/16) and external IPs on one or more blacklists.
   Communications prevented, dropped, denied and rejected by the firewall are excluded.", "Not Defined"
   "SLA report", "This report contains the Service Level Agreement values found during the reporting period.", "Monthly"
