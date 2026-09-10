Before starting the upgrade, carefully read the latest release notes on `NetEye's blog <https://www.neteye-blog.com/blog/category/release-notes-2/>`_ and check the features that will change or be deprecated.

#. All NetEye packages installed on a currently running version must be updated according to the
   :ref:`update procedure <update-procedure>` prior to running the upgrade.

#. NetEye must be up and running in a healthy state.

#. .. include:: /references/update-upgrade/update/free-disk-space.inc.rst

#. .. include:: /references/update-upgrade/update/elastic-prerequisites.inc.rst

#. Starting with |ne| 4.50, NetEye services are progressively moving to Kubernetes to improve
   scalability, security, resource management and the speed of updates.

   To enable these benefits and allow Kubernetes to retrieve the container images required
   during the upgrade and subsequent updates, ensure that all |ne| nodes can reach the
   following domains over HTTPS (TCP port 443) before upgrading to |ne| 4.50:

   The listed intended uses are examples and are not exhaustive.

   .. csv-table::
      :header: "Domain", "Port", "Intended Use"
      :widths: 25, 15, 60

      "ghcr.io", "443 TCP", "|ne| container images (e.g. the |ne| Operator)"
      "quay.io", "443 TCP", "Third-party container images (e.g. operators distributed through OperatorHub)"
      "docker.io", "443 TCP", "Third-party container images (e.g. the OpenTelemetry Collector and Alpine Linux)"
      "index.docker.io", "443 TCP", "RKE2 container images (e.g. Kubernetes control plane components)"
      "docker.elastic.co", "443 TCP", "Elastic container images (e.g. the EDOT Collector; only for installations with the Elastic Stack)"
