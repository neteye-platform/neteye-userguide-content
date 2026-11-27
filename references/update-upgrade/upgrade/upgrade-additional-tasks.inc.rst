Keycloak in Kubernetes and the |ne| Operator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After a successful upgrade, you will be required to perform and verify the migration of the Keycloak systemd/pcs
instance to the new Keycloak deployment in Kubernetes, which will be an upgrade prerequisite check for future releases.
To automate this process, the product ships 3 utility tools and scripts that allow you to perform the validation and
migration.

1. :command:`neteye config auth switch-to-kube`:
   This command will switch the authentication configuration from the systemd/pcs instance to the new Keycloak
   deployment in Kubernetes by updating the necessary configuration files. After successful execution, the systemd/pcs
   instance will be disabled, and you will be required to test the new environment to ensure that authentication is
   working as expected.
2. :command:`neteye config auth rollback-from-kube`:
   Should any issues arise with the new Keycloak deployment in Kubernetes, this command will allow you to roll back the
   authentication configuration to the previous systemd/pcs instance by restoring the necessary configuration files.
3. :command:`neteye config auth cleanup-keycloak-pcs`:
   Finally, once you are satisfied with the new Keycloak deployment, this command will clean up the old systemd/pcs
   instance by removing any remaining configuration files and data related to the previous Keycloak deployment. Backups
   of the old configuration files will be created in the :file:`/root/keycloak_upgrade_<timestamp>.bck` directory. This
   step is non-reversible, and you will not be able to roll back to the previous systemd/pcs instance
   after executing this command.

ZMQ Encryption between nProbe and ntopng
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| |neteye_version| ships ntopng 7, which can encrypt the ZMQ messages that nProbe sends to ntopng. New |ne|
installations encrypt these messages from the start. On installations upgraded from |neteye_previous_version|,
encryption stays disabled: ntopng and all nProbes, including the one running on |ne|, keep exchanging cleartext
messages. This gives you time to configure the *remote nProbes*, that is, nProbes installed outside |ne| that send
their flows to ntopng, as they cannot be configured automatically.

ZMQ encryption will be mandatory from the next |ne| version: an upgrade prerequisite check prevents the upgrade until
encryption is enabled.

The upgrade has already generated the key pair used for the encryption and stored it in
:file:`/root/.ntopng_zmq_keypair.json`. When you enable encryption, |ne| configures the nProbe running on |ne|,
including any additional ``nprobe@<name>`` instances, automatically. Only remote nProbes must be configured by hand. Whether
encryption is enabled is stored in :file:`/root/.ntopng_zmq_mode`, which contains ``disabled`` after the upgrade.

.. warning:: Once encryption is enabled, ntopng discards the messages of every nProbe not configured with the public
   key, and an nProbe configured with the key cannot talk to an ntopng with encryption disabled. Follow the steps below
   in the given order so that the flows of the remote nProbes are only lost for the short time between steps 5 and 6.

To enable ZMQ encryption:

#. Identify the remote nProbes. On a single node, or on the cluster node where ntopng is running, list the hosts
   connected to the ntopng ZMQ collector::

     neteye# ss -tn state established '( sport = :5556 )'

   Every peer address that is not a |ne| node belongs to a remote nProbe. The upgrade prerequisite check also lists them
   when it fails. Remote nProbes that are not running at the moment are not listed, so check your inventory too.

#. Read the public key::

     neteye# jq -r .public /root/.ntopng_zmq_keypair.json

   The public key is not confidential and can be copied to the remote nProbes. The ``secret`` value in the same file is
   the private key of ntopng: never copy it to other systems.

#. On every remote nProbe, add the option ``--zmq-encryption-key=<public_key>`` to its configuration, as described in
   :ref:`ntopng-remote-nprobes`, but **do not restart it yet**: until it is restarted, the nProbe keeps sending
   cleartext messages.

#. On every |ne| node, write ``enabled`` to the mode file. On clusters, this means every node except voting-only and
   Elastic-only nodes::

     neteye# echo enabled > /root/.ntopng_zmq_mode

   The file must have the same content on all the nodes, otherwise :command:`neteye install` fails.

#. Apply the configuration::

     neteye# neteye install

   This configures ntopng with the private key and the nProbes running on |ne| with the public key, and restarts them.

#. Immediately restart the remote nProbes, for example with :command:`systemctl restart nprobe`.

#. Verify that encryption is enabled by running the upgrade prerequisite check on every |ne| node::

     neteye# /usr/share/neteye/scripts/upgrade/upgrade-prerequisite-checks/060_check_ntopng_zmq_encryption_enabled.sh

   It must print ``[i] OK``. Then check in ntopng that the flows of the remote nProbes are received again.

If you cannot complete the configuration of the remote nProbes, you can temporarily go back by writing ``disabled`` to
the mode file on every node and running :command:`neteye install` again. In that case, remember to also remove the
``--zmq-encryption-key`` option from the remote nProbes and restart the service. Encryption must be enabled before
upgrading to the next |ne| version.
