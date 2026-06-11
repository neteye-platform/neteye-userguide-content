Secure Intracluster Communication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Security between the nodes in a cluster is just as important as
front-facing security. Because nodes in a cluster must trust each other
completely to provide failover services and be efficient, the lack of an
intracluster security mechanism means one compromised cluster node can
read and modify data throughout the cluster.

|ne| uses certificates signed by a Certificate Authority to ensure
that only trusted nodes can join the cluster, to encrypt data passing
between nodes so that third parties cannot tamper with your data, and to
allow for revocation for the certificates of each component in
each module.

See the list of natively clustered services in :ref:`clustering-and-single-purpose-nodes`.
