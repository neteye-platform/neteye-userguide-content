Report Scheduling Job
`````````````````````

The systemd service **icinga-reporting.service** is the job that is in
charge of performing the schedule of the Reports configured in the
Reporting module.

This means that, in case you need to have a look at the actions
performed to schedule your Reports, you can refer to this service, for
example with::

   journalctl -u icinga-reporting.service -f

The **icinga-reporting.service** service is bound to the
**php-fpm.service**, in such a way that on NetEye Cluster environments
the service will run only on one node.

.. note:: For debugging the failure of reporting scheduling jobs,
   check ``/neteye/shared/icingaweb2/log/icinga-reporting.log`` which
   contains the complete error logs occurred during the execution.

.. rubric:: Report generation


.. rubric:: Resource Report


SLM Resource Report is generated through icingacli using the dedicated
user **neteye_grafana_readonly** authenticated using a JWT token. All
required configurations are automatically performed during
``neteye install`` and must not be modified by the user.

Configuration performed are the following:

* A role **neteye_grafana_read_only_role** is added to **/neteye/shared/icingaweb2/conf/roles.ini**
* The JWT token is generated in
  **/neteye/shared/icingaweb2/conf/modules/analytics/jwt-tokens/neteye_grafana_readonly.jwt**
  alongside a key-pair. Public key can be found in
  **/neteye/shared/icingaweb2/conf/modules/neteye/jwt-keys/neteye-jwt.pub**
* A backend is added into **/neteye/shared/icingaweb2/conf/authentication.ini** to allow by
  default neteye login using JWT tokens validated using the aforementioned public key.

.. note:: During the Resource Report generation, some temporary users
   (i.e., ``neteye_report_temporary_XXXXXXXX``) are created in
   the ITOA Module. They are part of the process and will be
   removed as soon as the process completes.
