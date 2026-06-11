.. _elastic-agent:

Elastic Agent
~~~~~~~~~~~~~

|ne| officially supports the implementation and the installation of Elastic Agents on |ne|
:term:`Operative Node` and :term:`Single Purpose Node` types. Elastic Agent can be used to collect data from
local and external sources with a single unified agent per host. More information can be found in the
`official Elastic-Agent documentation <https://www.elastic.co/guide/en/fleet/current/fleet-overview.html>`__.

Elastic Agents in NetEye
````````````````````````

|ne| adopts Elastic Agents in order to enhance the monitoring of different hosts, using available
integrations given by the Elastic environment. A Fleet Server is implemented in the |ne| system in order to
manage the different configurations and integrations of multiple Elastic Agents that can be added to the
monitoring system. Make sure to respect all the :ref:`Requirements<individual-module-reqs>` in order to
ensure the correct operation of all Elastic Agent integrations.

In order to manage the agents that are present in the |ne| system, two different policies will be applied:

- **NetEye Operative Nodes:** This policy is applied to the agents running in the |ne| Operative nodes.
  It includes the following integrations:

  - *System:* Collects authentication logs of the Operative nodes.
  - *Fleet Server:* Coordinates all the enrolled Elastic Agents, updating and managing the policies across agents.
  - *APM Server:* Receives performance data from internal and external applications.

- **NetEye Single-Purpose Nodes:** This policy is assigned only to the Single-Purpose nodes of |ne|
  and includes the following integrations:

  - *System:* Collects authentication logs of the Single-Purpose nodes.

.. note:: Although it is allowed to include integrations into NetEye-managed policies,
   any additional integrations must be carefully considered as they affect the computational load on the system.

.. warning:: The retention of monitoring logs and metrics collected by Elastic Agent is set to be unlimited by default,
   while data generated from the different integrations have specific retentions managed by Elastic.
