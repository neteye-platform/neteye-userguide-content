Keycloak in Kubernetes and the |ne| Operator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After a successful upgrade, you will be required to perform and verify the migration of the Keycloak systemd/pcs
instance to the new Keycloak deployment in Kubernetes. To automate this process, the product ships 3 utility tools and
scripts that allow you to perform the validation and migration.

1. :command:`neteye config auth switch-to-kube`:
   This command will switch the authentication configuration from the systemd/pcs instance to the new Keycloak
   deployment in Kubernetes by updating the necessary configuration files. After a successful execution, the systemd/pcs
   instance will be disabled, and you will be required to test the new environment to ensure that the authentication is
   working as expected.
2. :command:`neteye config auth rollback-from-kube`:
   In case of any issues with the new Keycloak deployment in Kubernetes, this command will allow you to rollback the
   authentication configuration to the previous systemd/pcs instance by restoring the necessary configuration files.
3. :command:`neteye config auth cleanup-keycloak-pcs`:
   Finally, once satisfied with the new Keycloak deployment, this command will clean up the old systemd/pcs instance by
   removing any remaining configuration files and data related to the previous Keycloak deployment. Backups of the old
   configuration files will be created in the :file:`/root/keycloak_upgrade_<timestamp>.bck` directory. This step is non
   reversible, and you will not be able to rollback to the previous systemd/pcs instance after executing this command.
