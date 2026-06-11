
.. _ebp-setup-verify:

How to Setup the Automatic Verification of Blockchains
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This chapter illustrates the best practices for the setup of a secure and automatic
verification of El Proxy blockchains. We will setup an environment where the verification
of a specific blockchain is performed every day. Moreover, we will describe how we can notify
of the blockchain verification results in Icinga 2 via Tornado Webhook
Collectors and Tornado rules.

This guide is organized as follows: we first introduce the :ref:`ebp-verify-prereq` and then we describe
the :ref:`NetEye <ebp-verify-neteye-setup>` setup.
After that, we provide indications on how to :ref:`test <ebp-verify-testing>` the deployed configuration.


.. _ebp-verify-prereq:

Prerequisites
`````````````

  #. For the start you will need to have the file containing the initial key of the blockchain to be verified

  #. The automated verification needs to be set up on a machine which is external to the |ne|
     installation, accessible only by you, referred to as :abbr:`DPO (Data Protection Officer)` machine.
     The DPO machine must meet the following requirements:

     - Run a Linux distribution

     - Support `Docker <https://www.docker.com/>`__

       .. warning:: If you opt to use a Debian distribution, please verify that the `AppArmor` configuration is not
                    interfering with Docker, as executing the automated :command:`neteye dpo setup` could otherwise
                    trigger a `permission denied` error.

     - The DPO machine must be reachable, from the |ne| Master, on port 22 for the SSH connection during the setup procedure

       .. note:: The SSH connection will be necessary only when executing the setup command and will be performed using
                 password authentication. Credentials to access the DPO machine are **never** stored inside |ne|.

     - The DPO machine must reach the |ne| Master on port 9200 for contacting Elasticsearch.

     - The DPO machine must reach the :ref:`webhook_collector_host <tornado-webhook-collector-exec>` on port 443 to connect to the Tornado Webhook Collector.

     - The DPO machine must reach `DockerHub <https://hub.docker.com/r/neteye4/elproxy>`__ to be able to pull El Proxy container images

  #. The DPO machine must be a **known host** on the |ne| Master. This is needed for the setup procedure to safely connect
     to the DPO machine through SSH.
     Run the following command in order to add the DPO machine to the list of
     **known hosts** on the |ne| Master: :command:`ssh-keyscan <dpo-hostname> >> /root/.ssh/known_hosts`.

  #. The DPO machine and the |ne| installation must be configured in the same timezone

  #. The backup of the DPO machine must be performed on the customer's part

.. note:: If you would like to setup the automatic verification of the blockchain on a Windows system,
          you can download the Docker image from `DockerHub <https://hub.docker.com/r/neteye4/elproxy>`__
          and perform a manual setup of the automatic verification. For more information,
          please refer to the official channels: sales, consultants, or |support|_.

.. _ebp-verify-neteye-setup:

Verification Setup
``````````````````

**Step 1. Configure the blockchain verification**

Configuration is to be carried out on the |ne| Master in the :file:`/etc/neteye-dpo` JSON-formatted file.

As an example, we can configure, using the DPO *root* user on the *dpo-host* DPO machine, the verification of a
blockchain having a *6_months* retention. The blockchain belongs to the *test_tenant* and we would like the verification
to run each day at 7PM, sending the results to a Tornado Webhook Collector hosted at *satellite1* with the *test_token* token.
In this case, the configuration in the :file:`/etc/neteye-dpo` file is to be built as follows:

   .. code:: json

         {
           "dpo_host": "dpo-host",
           "dpo_user": "root",
           "blockchains_verification": [
             {
               "tenant": "test_tenant",
               "retention": "6_months",
               "tag": "0",
               "webhook_host": "satellite1",
               "webhook_token": "test_token",
               "cron_scheduling": {
                 "minute": "0",
                 "hour": "19",
                 "day": "*",
                 "month": "*",
                 "week_day": "*"
               }
             }
           ]
         }

For looking up a full list of configuration file attributes please consult :ref:`neteye-dpo-setup` section.

**Step 2. Run the** :command:`neteye dpo setup` **command**

The command should be run from the |ne| Master.
Upon running the command, you will be prompted to provide the password of the user,
specified in the configuration. This will allow you to connect to the DPO machine
and the initial key of the blockchain.

   .. note:: If you are not using the `root` account, you will be asked also for the password to be used to perform
             some actions with :command:`sudo`. If the password equals to the one used to connect via SSH to the DPO machine,
             you can press enter to use it also for the commands execution.

The command will connect to the DPO machine and set up the verification launching a container for each verification,
which will then be performed at the specified schedule.

   .. note:: Among the different actions performed by the command, this also tries to install Docker on the system.
             However, the automatic installation may fail for some OS which require a more specific installation path.
             If the command fails due to this error, please manually install Docker on the system and then
             run the command with the `--skip-tags install_docker_packages` option:

             .. code::

               neteye dpo setup --skip-tags install_docker_packages

**Step 3. Configure a** :ref:`tornado-webhook-collector-exec`

Tornado Webhook Collector is to be configured on either the |ne| Master, or a |ne| Satellite.
It will take care of receiving El Proxy blockchain verification result from the DPO
machine and forwarding it to Tornado, which will then set an Icinga 2 status.

   #. On the node where the Tornado Webhook Collector is running, create the file
      :file:`/neteye/shared/tornado_webhook_collector/conf/webhooks/elproxy_verification.json`

   #. Set its content to:

      .. code:: json

         {
           "id": "elproxy_verification",
           "token": "<webhook_token>",
           "collector_config": {
             "event_type": "elproxy_verification",
             "payload": {
               "data": "${@}"
             }
           }
         }

   #. Restart the Tornado Webhook Collector service to load the webhook

**Step 4. Configure a Rule in Tornado to set a status in Icinga 2**

Via |ne| Tornado GUI, create a Rule that matches the Events with type
``elproxy_verification`` and executes an Action of type :ref:`SMART_MONITORING_CHECK_RESULT <tornado-smartmon-check-executor>`,
where we set as ``check_result -> exit_status`` the value ``event.payload.data.exit_status`` and as
``check_result -> plugin_output`` the value ``event.payload.data.output``.

Moreover, if you are sending the results of the verification of multiple blockchains to the same Tornado Webhook Collector,
you can filter them by using the ``event.payload.data.tenant``, ``event.payload.data.retention`` and ``event.payload.data.tag``
values.

Configure the rest of the Rule as you prefer.



.. _ebp-verify-testing:

Testing the Configuration
`````````````````````````

Now everything should be configured correctly. To test your configuration,
on the DPO machine, you can :ref:`elproxy-trigger-manual-verification-container`.

.. _elproxy-trigger-manual-verification-container:

Manually trigger a pre-configured automatic verification
````````````````````````````````````````````````````````

As a user I want to be able to manually trigger a verification even though the automatic verification has already
been :ref:`configured <ebp-setup-verify>`, for example to immediately report the status of
the blockchain to the management.

#. Connect to the DPO machine using the same credentials specified when performing the setup of the
   :ref:`automatic verification <ebp-setup-verify>`.

   .. note:: If you connected to the DPO machine with a user different from `root`,
             run the following commands using :command:`sudo`

#. Open a shell in the verification container you would like to trigger manually. To do so, run the
   :command:`docker exec -it elproxy-verify-<tenant>-<retention>-<tag> bash` where the *tenant*, *retention* and *tag*
   match the properties of your blockchain.

   .. hint:: To check which verification containers are running on the DPO machine you can execute the :command:`docker ps` command

#. Extract the verification command from the cron file defining its schedule, located at :file:`/etc/cron.d/elproxy-verify-<tenant>-<retention>-<tag>`
   For example, for the blockchain defined in  :ref:`automatic verification <ebp-setup-verify>`, the file, located at
   :file:`/etc/cron.d/elproxy-verify-test_tenant-6_months-0` will have the following content:

   .. code::

      0 19 * * * root /bin/elproxy_verify_blockchain_and_send_result test_tenant 6_months 0 satellite1 test_token 1000

   In this case, the verification command will be :command:`/bin/elproxy_verify_blockchain_and_send_result test_tenant 6_months 0 satellite1 test_token 1000`
#. Run the extracted verification command.

   |

   *Expected results:*

   |

   The verification is performed and the result is sent to the configured
   Tornado Webhook Collector. The output of the command is saved in the :file:`/root/elproxy-verification/logs` folder,
   with the file being identified by the verified blockchain and the current timestamp.
