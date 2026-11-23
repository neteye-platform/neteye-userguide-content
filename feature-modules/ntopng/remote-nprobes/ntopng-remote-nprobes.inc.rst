Besides the nProbe running on |ne|, nProbes installed on other systems, called *remote nProbes*, can send their flows
to ntopng. ntopng acts as ZMQ collector on TCP port 5556 (see :ref:`table-ntopng-port-req`), and every nProbe
connects to it as the connection initiator.

The ZMQ messages between nProbe and ntopng are encrypted. During :command:`neteye install`, |ne| generates a dedicated
key pair and stores it in :file:`/root/.ntopng_zmq_keypair.json`: ntopng uses the private key (``secret``), while each
nProbe must be configured with the public key (``public``). |ne| automatically configures the nProbe running on |ne|, including any
additional ``nprobe@<name>`` instances. Remote nProbes must be configured by hand.

.. TODO(NTOPNG-90): remove in NetEye 4.52, together with the update/upgrade Additional Tasks: every installation
   reaching 4.52 has ZMQ encryption enabled.

.. note:: On installations upgraded from |ne| 4.50, ZMQ encryption may still be disabled, that is,
   :file:`/root/.ntopng_zmq_mode` contains ``disabled``. In this case, omit the ``--zmq-encryption-key`` option below,
   otherwise ntopng cannot receive the flows of the nProbe, and add it when enabling encryption as described in the
   *Additional Tasks* of the :ref:`upgrade procedure <upgrade-additional-steps-single>`.

To configure a remote nProbe:

#. On |ne|, read the public key::

     neteye# jq -r .public /root/.ntopng_zmq_keypair.json

#. In the configuration file of the remote nProbe, for example :file:`/etc/nprobe/nprobe.conf`, add the following
   options, where ``<neteye_fqdn>`` is the FQDN through which the nProbe reaches |ne|. On a cluster, use the FQDN of
   the Cluster IP (ntopng always runs on the node holding the ``cluster_ip`` resource)::

     --zmq=tcp://<neteye_fqdn>:5556
     --zmq-probe-mode
     --zmq-encryption-key=<public_key>

   If you set the options on the command line instead, enclose the key in single quotes: it can contain characters
   that have a special meaning for the shell.

#. Restart the nProbe, for example with :command:`systemctl restart nprobe` on Linux.

#. Check in ntopng that the flows of the nProbe are received.

The key pair is kept across updates and upgrades and is the same on all the nodes of a cluster. Do not edit the
:file:`zmq_encryption.conf` files that |ne| generates in :file:`/neteye/shared/ntopng/conf/ntopng.conf.d/` and
:file:`/neteye/shared/nprobe/conf/nprobe.conf.d/`, as :command:`neteye install` overwrites them.

.. seealso:: The `nProbe documentation <https://www.ntop.org/guides/nprobe/index.html>`__ describes the ZMQ options
   in detail.
