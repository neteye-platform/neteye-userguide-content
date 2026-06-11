The IDO database is not removed automatically during the upgrade, so if you want to delete it you have to run the following commands:

   .. code:: bash

      mysql -e "DROP DATABASE icinga;"
      mysql -e "DROP USER 'icinga'@'localhost';"

If you have the Elastic Stack installed, the ``retention-policy-neteyelocal``
service will be removed during the upgrade procedure. After the upgrade is
complete, you will need to Director deploy to make the changes effective.
