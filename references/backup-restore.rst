.. _backup-restore:

Backup and Restore
==================

This section provides instructions on how to back up and restore your NetEye configuration and data. Regular backups are essential to prevent data loss and ensure quick recovery in case of system failures or other issues.

.. _backup-restore-services:

Supported services
------------------

Currently, the backup and restore procedures support out-of-the box the following services:

* Core:
    - NetEye base configuration files, e.g. :file:`/etc/neteye-cluster`, :file:`/etc/neteye-satellite.d` and SSH keys
    - MariaDB
    - Tornado
    - Keycloak
    - Icinga 2
    - Grafana
    - All Icinga Web 2 modules installed on your system, including those coming from the installation of additional feature modules.
* Elastic Stack:
    - Elasticsearch
    - Kibana

.. _backup-restore-providers:

Supported providers
-------------------

The backup and restore procedures currently support AWS S3 buckets as remote storage providers. Support for additional providers will be added in future releases.

.. _backup-restore-s3-configuration:

Configuring S3 buckets for backup and restore
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to use an S3 bucket as remote storage for backup and restore operations, you need to perform the following steps:

1. Create an S3 bucket in your AWS account.
2. Create a KMS key for encrypting the backup data server side. To do so, please follow the official `AWS documentation <https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html>`_.
3. Using the AWS Console, create an `IAM user <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html>`_. When is time to assign permissions,
   choose the option to attach existing policies directly, and create a new policy with the permissions as outlined below
   (replace the placeholders with your actual bucket name, region, account id, and key id):

    .. code:: json

        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "VisualEditor0",
                    "Effect": "Allow",
                    "Action": [
                        "s3:ListBucketMultipartUploads",
                        "s3:ListBucketVersions",
                        "s3:ListBucket",
                        "s3:GetBucketLocation",
                        "s3:CreateBucket"
                    ],
                    "Resource": [
                        "arn:aws:s3:::<your-bucket-name>"
                    ]
                },
                {
                    "Sid": "VisualEditor1",
                    "Effect": "Allow",
                    "Action": [
                        "s3:PutObject",
                        "s3:GetObject",
                        "s3:AbortMultipartUpload",
                        "s3:DeleteObject",
                        "s3:ListMultipartUploadParts",
                        "s3:CreateBucket"
                    ],
                    "Resource": [
                        "arn:aws:s3:::<your-bucket-name>/*"
                    ]
                },
                {
                    "Sid": "AllowUseOfKMSKey",
                    "Effect": "Allow",
                    "Action": [
                        "kms:GenerateDataKey",
                        "kms:Decrypt"
                    ],
                    "Resource": [
                        "arn:aws:kms:<region>:<account-id>:key/<key-id>"
                    ]
                }
            ]
        }


4. From the IAM user management console, generate a new access key for the user. You will need the access key ID and secret access key to configure the backup and restore procedures in NetEye.


.. _backup-restore-configuration:

How to configure the NetEye backup and restore and take scheduled backups
-------------------------------------------------------------------------

The configuration of the backup and restore procedures is done via YAML files in the :file:`/etc/neteye-backup.d` directory.

The basic and common configuration can be found in the :file:`/etc/neteye-backup.d/common.yaml` file.

It is so possible to configure various aspects of the backup and restore procedures, such as:

* Which items to include in the backup (configurations, data and logs)
* The schedule for automatic backups.

  .. note:: Please note that for backup schedules simple ranges (e.g., "0-1") are supported while step values (e.g., "*/5") are **NOT** supported.

* The remote storage provider settings (e.g., S3 bucket details, access keys, encryption settings)
* The backup node in a cluster setup, which defaults to the first node specified in the cluster configuration file (:file:`/etc/neteye-cluster`).
  This node is responsible to perform the scheduled backups and collect the data from the other nodes in the cluster.

.. warning:: One of the most critical settings is the ``throttle_parallel_jobs``. This setting is used to throttle the
   backup jobs running in parallel, to avoid overloading the system and ensure that the backup process does not impact
   the performance of the services. It is essential to set this value according to the resources available on your
   system and the amount of data to be backed up, to ensure a successful and smooth backup process.

Besides the common configuration file, a specific configuration file for each supported service is present in the
corresponding feature module directory, e.g., :file:`/etc/neteye-backup.d/elastic-stack/elasticsearch.yaml`.

In the specific configuration file it is possible to override some of the common settings, such as which items to include in the backup for that specific service
and also specify service-specific settings, such as the Elasticsearch indices to include in the backup.

After the configuration files have been set up, you can apply the configuration using the following command, that can be run on one of the cluster nodes:

.. code:: bash

   neteye backup config apply

This command will validate the configuration files and apply the settings to the backup and restore procedures, scheduling also the automatic backups as specified on the backup node.


.. _backup-restore-manual-backup:

How to take a manual backup
---------------------------

Sometimes, you may want to take a manual backup outside the scheduled automatic backups. To do so, you can use the following command, on a node of your installation, to take a manual backup:

.. code:: bash

   neteye backup run


Furthermore, in case you would like to take a manual backup of a specific service only, you can use the ``--restrict-services-to`` option, as shown in the example below:

.. code:: bash

   neteye backup run --restrict-services-to core


.. _backup-restore-restore:

How to restore a backup
-----------------------

.. warning::

   Restoring a backup will overwrite the current configuration and data of the selected services
   with those from the **MOST RECENT** backup available in the remote storage.

.. warning::

   The restore operation is primarily intended to recover from failures or data loss on the
   system from which a backup was taken.

   It is possible to restore a backup on a different system, if the following conditions are met:

    * The NetEye version on the target system is the same as the one on the source system from which the backup was taken.
    * The target system has already performed the initial configuration using the :command:`neteye install` command.
    * The number of nodes in the cluster (if applicable) is the same on both systems and the hostnames of the nodes match those in the backup.
    * The target system has network connectivity to the remote storage where the backup is stored.
    * The target system has sufficient resources (disk space, memory, etc.) to accommodate the restored data.

    Furthermore, please note that at the moment restoring also MariaDB on a different system will lead to failed authentication
    by the services that are currently not supported, such as IcingaWeb2 modules. For this reason, it is recommended to
    restrict the restore operation to the supported services only, e.g., using the ``--restrict-services-to`` option explained below.
    In case also the restore of MariaDB is needed, it is possible to restore the single databases manually as explained in the
    `How to restore a single database from a full backup`_ section below.

To restore a backup, the first step is to ensure the backup configuration that will allow the
connection to the remote storage where the backups are correctly configured.

This can be done either by manually editing the configuration files in the :file:`/etc/neteye-backup.d` directory,
in particular the :file:`/etc/neteye-backup.d/common.yaml` file, or by restoring manually the backup configuration files from a previous backup.

By default, the backup configuration files are stored in the `tar.gz` archive that can be found in the ``core/conf`` folder inside the remote storage.

After that, the restore operation can be initiated, on a node of your installation, using the following command:

.. code:: bash

   neteye backup restore

As mentioned also above, this command will restore the most recent backup available in the remote storage.

Also in this case, it is possible to restore a specific service only by using the ``--restrict-services-to`` option, as shown in the example below:

.. code:: bash

   neteye backup restore --restrict-services-to core


Monitoring the backup status through Icinga2
--------------------------------------------

It is absolutely important to monitor the status of the backup operations to ensure that backups are being performed successfully and that the data is safe.

For this reason, |ne| provides built-in Icinga2 checks that monitor the status of the last backup operation.

In particular, after the first `neteye backup config apply` command is run, two new services will be created in Icinga2:

* `neteye-backup-status`: This service checks whether the last backup operation was successful or not.
* `elasticsearch-backup-status`: This service, available only if the Elastic Stack feature module is installed, checks whether the last Elasticsearch backup operation was successful or not.
                                 The service was separated from the core backup status to allow a more precise monitoring of the Elasticsearch backup, which is performed by Elasticsearch
                                 itself using the Snapshot and Restore functionality, based on the configured schedule.

It will be sufficient to perform a director configuration deployment to apply the new services and start monitoring the backup status.

Service specific scenarios
--------------------------

NetEye Core
~~~~~~~~~~~

Backup of NetEye password files
```````````````````````````````

The backup procedure for NetEye Core includes the backup of the password files used by NetEye, which are stored in the
:file:`/root/.pwd_*` files.

.. warning:: During the :command:`restore` procedure, these files are not restored, to avoid overwriting the newly
            generated password in case of restore on a different system.

MariaDB
~~~~~~~

Scope of MariaDB Backups
````````````````````````

In order to perform a backup of MariaDB different tools and techniques can be used, depending on the specific requirements and constraints of the environment.

To support out of the box *Disaster Recovery* scenarios, the backup and restore procedures for MariaDB in |ne|
are designed to perform a full backup and restore of all databases used by |ne|, using under the hood the `mariadb-backup` utility.

.. note:: If you regularly need to back up and restore individual databases, we recommend using `mysqldump`,
          which is already installed on |ne|. For further optimization, you can also consider
          pairing it with a tool such as `mydumper <https://github.com/mydumper/mydumper>`_.

Furthermore, to have a faster backup procedure, by default |ne| exploits the functionality of `mariadb-backup` to perform
an incremental backup. This means that after the first full backup, only the changes made since the last backup are saved in subsequent backups,
reducing the amount of data to be transferred and stored.

However, since the restore procedure always requires the presence of the last full backup and all subsequent incremental backups to be applied,
to avoid slowing it down considerably, by default a new full backup is taken every `6` incremental backups.
This behavior can be customized by modifying the `partial_backups` setting in the :file:`/etc/neteye-backup.d/core/mariadb.yaml` configuration file.


How the backup is performed
```````````````````````````

The backup procedure for MariaDB in |ne| uses the `mariadb-backup` utility to create a consistent snapshot of all databases used by |ne|.

.. note:: Given that the `mariadb-backup` utility performs a local backup, the designed node must be
          a node running MariaDB (voting-node excluded) and with enough space in the backup working directory.
          By default, this is the first node in the `/etc/neteye-cluster` file with the `mariadb` role.

Following the `official documented procedure <https://mariadb.com/docs/galera-cluster/galera-management/general-operations/backing-up-a-mariadb-galera-cluster/>`_,
the backup is taken by performing the following steps:

1. Some backup preparation steps, which do not impact the Galera cluster in any way
2. Removal of the target backup node
   from the load balancer to avoid receiving traffic during the backup process
3. During the backup, the target node may be de-synced to avoid affecting Galera cluster performance.
   Note that this does not mean that the node will be de-synced for sure, but only that it is allowed to be de-synced if the backup process impacts its performance
4. The actual backup is taken using the `mariadb-backup` utility
5. The target backup node is re-added to the load balancer and the re-sync with other nodes, if needed, is ensured
6. Some backup finalization steps, which do not impact the Galera cluster in any way

.. note:: In case the backup fails the node is re-added and re-sycned to the Galera cluster to avoid leaving it in an inconsistent state.


Backup encryption
`````````````````

For security reasons, the backup data for MariaDB is encrypted using a random password generated during the backup process.

The password is stored inside the :file:`/root/.pwd_mariadb_backup` file and is not included in the backup data itself,
to ensure the backup data remains secure even if the remote storage is compromised.

.. warning::

   It is essential to keep the password file safe and secure, as it is required to restore the backup data.
   Losing this file will make it impossible to decrypt and restore the backup. For this reason,
   it is recommended to save a copy of this file in a secure location, separate from the backup data.

How to restore a single database from a full backup
```````````````````````````````````````````````````

The current backup and restore procedures for MariaDB in |ne| are designed to perform a full backup and restore of all databases used by |ne|.

However, starting from the full backup taken by |ne| it is possible to extract and restore a single database manually, by following these steps:

- Run the |ne| restore command with the following extra option to prepare the backup data without performing the actual restore:

  .. code:: bash

     neteye backup restore --restrict-services-to mariadb -e mariadb_prepare_restore_only=true

- Run a MariaDB 10.11 container mounting the prepared backup data directory as a volume. The path to this directory is composed by the directory
  specified in the `/etc/neteye-backup.d/common.yaml` configuration file under the `working_dir` key, followed by the `mariadb/restore-full` subdirectory.
  By default, this corresponds to the `/neteye/backup/mariadb/restore-full` path.

    .. code:: bash

        podman run -d -v /neteye/backup/mariadb/restore-full:/var/lib/mysql:Z --name mariadb-backup mariadb:10.11.10

- From inside the container use, `mysqldump` to export the desired database to a SQL file, directly inside the mounted backup data directory:

    .. code:: bash

        podman exec -it mariadb-backup bash
        mysqldump -u root --password=<your-root-password> <database-name> > /var/lib/mysql/<database-name>-restore.sql
        exit

- Outside the container, restore the SQL file to your MariaDB instance using the `mysql` command:

    .. code:: bash

        mysql -u root <database-name> < /neteye/backup/mariadb/restore-full/<database-name>-restore.sql


How to force a full backup
``````````````````````````

We know that, in some cases, especially before some impacting operations, it could be useful to take a full backup of MariaDB
manually, regardless of the configured full backup interval.

To do so, you can use the following command, on a node of your installation, to take a manual full backup of MariaDB:

.. code:: bash

   neteye backup run -e mariadb_force_full_backup=true

This full backup will also reset the incremental backup chain, so that subsequent backups will be incremental
until the next full backup is taken, either manually or automatically based on the configured interval.

Elasticsearch
~~~~~~~~~~~~~

Configuring Elasticsearch indices to be included in the backup
``````````````````````````````````````````````````````````````

By default, the :command:`backup` procedure will include all Elasticsearch indices in the backup. However, it is
possible to specify a list of indices to be included in the backup by modifying the
`elasticsearch.policy.config.indicies` setting in the :file:`/etc/neteye-backup.d/elastic-stack/elasticsearch.yaml`
configuration file.

.. note: This is an inclusion list, so only the indices specified in the list (and the ones matching glob patterns) will
        be included in the backup, while all the others will be excluded. To exclude specific indices from the backup,
        you can leverage the `-{my-custom-index-or-pattern}` syntax.
