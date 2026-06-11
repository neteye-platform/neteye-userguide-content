.. _rhel-os:

Underlying Operating System
---------------------------

Since release of |ne| 4.23, we build our product on top of **Red Hat Enterprise Linux 8**
(RHEL 8). This allows us to benefit from the feature an utilities Red Hat provide with RHEL
and pass that onto our clients.

.. _rhel-lifecycle:

RHEL 8 Life Cycle
~~~~~~~~~~~~~~~~~

The RHEL 8 Life Cycle covers at least 10 Years. In the first five years of its lifetime,
RHEL 8 gets full support from Red Hat. This means all packages will receive security updates
and bug fixes, as well as selected software enhancements at the discretion of Red Hat. The
focus for minor releases during this phase lays on resolving defects of medium or higher
priority. Full Support is projected to end on **May 31, 2024**.

After that, RHEL 8 will transition into the **Maintenance Support Phase**. In this phase the
packages will still get high priority security and bug fixes, but no minor version
upgrades or enhancements. The Maintenance Support Phase is projected to end on **May 31, 2029**.

The last phase is the **Extended Life Phase**. In this phase, Red Hat no longer provides updated
installation images. The technical support is limited to the pre-existing installations and
no updates will be rolled out. To keep support and updates into this last phase, there exist
*Support Add-ons* for the subscription, to guarantee extra support even after the end of the
Maintenance Support Phase. The Extended Life Phase is projected to end on **May 31, 2031**.

.. seealso:: For more information on *RHEL 8 Life Cycle*
   visit official `Red Hat Customer Portal <https://access.redhat.com/support/policy/updates/errata>`__



.. _red-hat-insights:

Red Hat Insights Integration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| and the Red Hat subscription are also integrated with `Red Hat
Insights <https://www.redhat.com/en/technologies/management/insights>`_, which gives a quick
overview of all systems registered under our licenses. It also lists some |ne| specific data for each
server, like the role, server and deployment type, serial number, |ne| version, installed |ne| dnf
groups and more.

Registration is done during ``neteye install`` but first you need to run the following command
in order to generate the correct tags that will be associated with the machine.

.. code:: bash

   neteye# neteye node tags set

.. seealso:: For more information see the section on the :ref:`neteye node tags command <neteye-node-tags>`.

.. _rhel-security-guarantees:

Security Guarantees
~~~~~~~~~~~~~~~~~~~

Red Hat guarantees, that to its knowledge, the Software does not, at the time of delivery to
you, include malicious mechanisms or code for the purpose of damaging or corrupting the Software;
and the Services will comply in all material respects with laws applicable to Red Hat as the
provider of the Services.

The *Red Hat Open Source Assurance program* furthermore protects the clients from the effects of
an intellectual property infringement claim on any Red Hat products. This may include: (i)
replacing the infringing portion of the software, (ii) modifying the software so that its use
becomes non-infringing, or (iii) obtaining the rights necessary for a customer to continue use
of the software.

.. seealso:: These guarantees are stated in the Red Hat Enterprise Agreement
   https://www.redhat.com/en/about/agreements

.. _rhel-security-fixes:

Security Fixes
~~~~~~~~~~~~~~

Red Hat will provide `backports <https://access.redhat.com/solutions/57665>`_ of security fixes
until the EOL of the package. However the package name does not always follow the semantic versioning
conventions of the upstream source.  Red Hat will only increase the revision number of their
packages when backporting bug fixes. That may lead to some confusion if the upstream release was
patched in a newer version, than the one provided by Red Hat. If external auditing tools rely solely
on the version of the package, this may also lead to false positives.


Red Hat and CVEs
````````````````

Red Hat adds the CVE names to all *Red Hat Security Advisories* for easier cross-referencing since 2001.
This makes it easy to check if a system is affected by a certain CVEs. Red Hat provides the
`Red Hat CVE Database <https://access.redhat.com/security/security-updates/cve>`_ where one can
look up releases for a certain CVE. RHEL also provides the `oscap command-line utility <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/security_hardening/scanning-the-system-for-configuration-compliance-and-vulnerabilities_security-hardening#scanning-the-system-for-vulnerabilities_vulnerability-scanning>`_
which scans the system for known vulnerabilities and policy violations. CVEs for which RHEL issues
a Security Advisor can be viewed in the `Vulnerability service <https://console.redhat.com/insights/vulnerability/cves>`_.

.. seealso:: Red Hats CVE Q&A https://access.redhat.com/articles/2123171
