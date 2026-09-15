Before starting the upgrade, carefully read the latest release notes on `NetEye's blog <https://www.neteye-blog.com/blog/category/release-notes-2/>`_ and check the features that will change or be deprecated.

#. All NetEye packages installed on a currently running version must be updated according to the
   :ref:`update procedure <update-procedure>` prior to running the upgrade.

#. NetEye must be up and running in a healthy state.

#. .. include:: /references/update-upgrade/update/free-disk-space.inc.rst

#. .. include:: /references/update-upgrade/update/elastic-prerequisites.inc.rst

#. Starting with |ne| 4.50, |ne| services are progressively moving to Kubernetes to improve
   scalability, security, resource management and the speed of updates.

   To enable these benefits and allow Kubernetes to retrieve the container images required
   during the upgrade and subsequent updates, ensure that all |ne| nodes can reach the
   following domains over HTTPS (TCP port 443) before upgrading to |ne| 4.50:


     .. csv-table::
        :header: "Domain", "Port", "Intended Use"
        :widths: 25, 15, 60

        "ghcr.io", "443 TCP", "GitHub container images"
        "api.github.com", "443 TCP", "GitHub container images"
        "pkg-containers.githubusercontent.com", "443 TCP", "GitHub container images"
        "quay.io", "443 TCP", "Quay container images"
        "cdn01.quay.io", "443 TCP", "Quay container images"
        "docker.io", "443 TCP", "Docker Hub container images"
        "hub.docker.com", "443 TCP", "Docker Hub container images"
        "auth.docker.io", "443 TCP", "Docker Hub container images"
        "index.docker.io", "443 TCP", "Docker Hub container images"
        "registry-1.docker.io", "443 TCP", "Docker Hub container images"
        "production.cloudfront.docker.com", "443 TCP", "Docker Hub container images"
        "\*.cloudflarestorage.com", "443 TCP", "Container images"
        "rpm.rancher.io", "443 TCP", "Rancher packages"
        "docker.elastic.co", "443 TCP", "Elastic container images (only with the Elastic Stack)"
        "docker-auth.elastic.co", "443 TCP", "Elastic container images (only with the Elastic Stack)"
