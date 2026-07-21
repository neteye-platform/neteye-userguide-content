Kubernetes
----------

Since |ne| 4.49, |ne| integrates a Kubernetes distribution to manage the deployment of some of its components.
This integration represents a step towards the transformation of |ne| into a cloud-native application, which is the long-term goal.

The chosen Kubernetes distribution is `RKE2 <https://docs.rke2.io/>`_, which is a CNCF-certified Kubernetes distribution.
RKE2 is designed to be secure by default, with a focus on simplicity and ease of use.

|ne| completely manages the RKE2 installation and configuration, so that the user does not have to worry about the underlying Kubernetes infrastructure.

.. _kubernetes-roles:

Kubernetes roles
~~~~~~~~~~~~~~~~

|ne| offers two types of Kubernetes roles to be able to more efficiently manage the compute resources of the |ne| installation:

- **Kubernetes Master**: This role is responsible for managing the Kubernetes cluster and its components. It runs the Kubernetes API server, controller manager, scheduler, and etcd database.
  The master node is also responsible for managing the deployment of |ne| components on the worker nodes.
- **Kubernetes Worker**: This role is responsible for running the |ne| components and workloads. Worker nodes receive instructions from the master node and execute the tasks accordingly.

A node can have any possible combination of the two roles, with the only constraint of having at least three master nodes and two worker nodes in cluster installations.

Kubernetes is also supported and configured on Single Node installations, where the single node has implicitly both the master and worker roles.

Kubernetes installed tools
~~~~~~~~~~~~~~~~~~~~~~~~~~

The following tools are installed and configured to simplify the management of the Kubernetes cluster and its components:

- **kubectl** (`doc <https://kubernetes.io/docs/reference/kubectl/>`_): The command-line tool for interacting with the Kubernetes cluster.
- **k9s** (`doc <https://k9scli.io/topics/commands/>`_): A terminal-based UI for managing Kubernetes clusters.
- **helm** (`doc <https://helm.sh/>`_): A package manager for Kubernetes that simplifies the deployment and management of applications on the cluster.
- **crictl** (`doc <https://github.com/kubernetes-sigs/cri-tools>`_): A command-line interface for interacting with container runtimes, such as containerd, which is used by RKE2.

The most common tools for managing the Kubernetes cluster are kubectl and k9s, which are installed on all nodes of the |ne| installation and
automatically configured to connect to the local Kubernetes cluster.

For example, to check the status of the Kubernetes cluster, you can run the following command on any node:

.. code-block:: bash

   kubectl get nodes

or, alternatively, you can use k9s to get a more user-friendly view of the cluster status:

.. code-block:: bash

   k9s

and then in the k9s interface, you can navigate to the "Nodes" view to see the status of all nodes in the cluster by writing `:nodes` in the command bar and pressing Enter.

For more information on their usage and options, please refer to the official documentation.
