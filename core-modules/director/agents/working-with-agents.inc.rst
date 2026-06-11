
Working with Icinga 2 Agents can be quite tricky, as each Agent needs
its own Endpoint and Zone definition, correct parent, peering host and
log settings. There may always be reasons for a completely custom-made
configuration. However, it is **strongly suggested** to use the **Director-
assisted** variant, which may save a lot of effort.

.. note:: In order to use all the functionalities of NetEye 4, use
   our :ref:`Icinga2 Packages <icinga2-packages>` userguide to get
   icinga2 packages for different OS/distributions via the NetEye repositories for
   Icinga2 agent installation.

Preparation
```````````

Agent settings are not available for modification directly on a host
object. This requires you to create an “Icinga Agent” template. You
could name it exactly like that; it’s important to use meaningful names
for your templates.

.. figure:: /core-modules/director/agents/img/agent-template.png
   :alt: Create an Agent template

   Create an Agent template

As long as you’re not using Satellite nodes, a single Agent zone is all
you need. Otherwise, you should create one Agent template per satellite
zone. If you want to move an Agent to a specific zone, just assign it
the correct template and you’re all done.

Usage
`````

Well, create a host, choose an Agent template, that’s it:

.. figure:: /core-modules/director/agents/img/create-agent-based-host.png
   :alt: Create an Agent-based host

   Create an Agent-based host

Once you import the “Icinga Agent” template, you’ll see a new “Agent”
tab. It tries to assist you with the initial Agent setup by showing a
sample config:

.. figure:: /core-modules/director/agents/img/show-agent-instructions-1.png
   :alt: Agent instructions 1

   Agent instructions 1

The preview shows that the Icinga Director would deploy multiple objects
for your newly created host:

.. figure:: /core-modules/director/agents/img/agent-preview.png
   :alt: Agent preview

   Agent preview

Create Agent-based services
```````````````````````````

Similar game for services that should run on your Agents. First, create
a template with a meaningful name. Then, define that Services inheriting
from this template should run on your Agents.

.. figure:: /core-modules/director/agents/img/agent-based-service.png
   :alt: Agent-based service

   Agent-based service

Please do not set a cluster zone, as this would rarely be necessary.
Agent-based services will always be deployed to their Agent’s zone by
default. All you need to do now for services that should be executed on
your Agents is importing that template:

.. figure:: /core-modules/director/agents/img/create-agent-based-load-check.png
   :alt: Agent-based load check

   Agent-based load check

Config preview shows that everything works as expected:

.. figure:: /core-modules/director/agents/img/agent-based-service-rendered-for-host.png
   :alt: Agent-based service preview

   Agent-based service preview
