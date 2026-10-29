.. _neteye-cra-security-risk-information:

Known Risks and Foreseeable Misuse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Annex II §5

The following deployment or operating conditions are known to create
significant cybersecurity risks. They are outside the intended use of |ne|.

* **Exposing the web interface or API to the public internet** without a
  :ref:`reverse proxy <howto-networking-proxy>`, network restriction or VPN
* **Continuing to operate a release that is no longer supported** -- such releases
  no longer receive security fixes; see our :ref:`Bugfix Policy <bugfix-policy>`
* **Keeping default or shared credentials** for administrative accounts, or
  reusing service account credentials across environments
* **Disabling authentication, TLS or the audit log** to simplify integration
* **Running the underlying operating system without vendor security updates**,
  or on a :ref:`release <neteye-bugfix-policy-os>` that has reached end of life
* **Granting broad module permissions** to users who only require read access,
  particularly for modules that can :ref:`execute commands <neteye-command-orchestrator-architecture>`
  on monitored hosts
* **Storing monitoring data with confidentiality requirements** without
  configuring *encryption at rest* on the underlying storage

If any of these conditions cannot be avoided, compensating controls should be
documented in the operator's own risk assessment.

|


.. _neteye-cra-security-config-information:

How Configuration Changes Affect Data Security
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Annex II §8b

Several configuration options change how |ne| handles data. Before applying
them, consider the effect on confidentiality, integrity and availability.

.. list-table::
   :header-rows: 1
   :widths: 35 65
   :width: 100%

   * - Change
     - Effect on data security
   * - Disabling TLS on an interface
     - Monitoring data and credentials are transmitted in clear text
   * - :ref:`Adding a satellite <neteye-satellite-install>`
     - Monitoring data is transmitted to and buffered on an additional node
       that must be secured to the same standard
   * - Federating authentication to an external IdP
     - Account lifecycle and password policy move to the IdP; access to |ne|
       depends on its availability and configuration
   * - Widening module permissions
     - Users may gain access to monitoring data or command execution beyond
       their role
   * - Changing retention settings
     - Longer retention increases the volume of data exposed by a compromise;
       shorter retention may conflict with audit requirements
   * - Disabling the audit log
     - Administrative actions are `no longer traceable <https://eur-lex.europa.eu/eli/reg/2018/1725/oj/eng#d1e4967-39-1>`__
   * - :ref:`Restoring a backup <backup-restore>`
     - The configuration state, including accounts and permissions, is reverted
       to the time of the backup

|
