
..  _service-level-objectives:

Security Bug Fix Service Level Objectives (SLO)
-----------------------------------------------

NetEye sets service level objectives for fixing security vulnerabilities based
on the security severity level and the affected package. The following timeframes
for fixing security issues in our product have been defined:

..  _standard-timeframe:

Standard Resolution Timeframe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following resolution timeframes are respected should a security vulnerability be discovered
within NetEye:

 * **Critical, High** severity bugs in product to be fixed within 90 days of being verified
 * **Medium, Low** severity bugs to be fixed in product within 180 days of being verified


Critical Vulnerabilities
~~~~~~~~~~~~~~~~~~~~~~~~

When a Critical security vulnerability is discovered by a third party or |witit| itself,
the latter will release a fix for the :ref:`current version <intro-neteye-releases>` of the product:

For example: If a bug is discovered in 4.29 (current version) and the fix is ​​released 90 days later
(current version after 90 days: 4.30) only 4.30 will be fixed.

.. note:: The bugfixes are not backported to versions prior to the one where the bug was fixed.
   This is why it is important to stay on the latest release for the version of the product you are using
   (this is best practice).

Non-critical vulnerabilities
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When a security issue of a High, Medium or Low severity level is discovered, NetEye will aim to release
a fix for the :ref:`current version <intro-neteye-releases>` of the product within the service level objectives
listed in :ref:`standard-timeframe`.

You should upgrade your installations when a bug fix release becomes available to ensure that
the latest security fixes have been applied.


..  _severity-levels:

Severity Levels
---------------

|ne| uses Common Vulnerability Scoring System (CVSS) as a method of assessing security risk
and prioritization for each discovered vulnerability.
CVSS is an industry standard vulnerability metric. You can learn more about CVSS at `FIRST.org <https://www.first.org/cvss/user-guide>`__.

|ne| security advisories include a severity level. It is based on our
self-calculated CVSS score for each specific vulnerability.

  * Critical
  * High
  * Medium
  * Low

For CVSS v3 |witit| uses the following severity rating system:

.. csv-table::
   :header: "CVSS V3 Score Range", "Severity in Advisory"

   "9.0 - 10.0", "Critical"
   "7.0 - 8.9", "High"
   "4.0 - 6.9", "Medium"
   "0.1 - 3.9", "Low"

In some cases, |ne| may use additional factors unrelated to CVSS score
to determine the severity level of a vulnerability. This approach is supported by the `CVSS v3.1 <https://www.first.org/cvss/v3.1/specification-document>`__.
Should this approach be taken, |ne| will describe which additional factors
have been considered and why upon publicly disclosing the vulnerability.

Below are a few examples of vulnerabilities which may result in a given severity level.
Please keep in mind that this rating does not take into account details of your installation
and are to be treated purely for a guide purpose.

**Severity Level: Critical**

Vulnerabilities that score in the critical range usually have most of the following characteristics:

  * Exploitation of the vulnerability likely results in root-level compromise
    of servers or infrastructure devices.
  * Exploitation is usually straightforward, in the sense that the attacker does not need
    any special authentication credentials or knowledge about individual victims,
    and does not need to persuade a target user, for example via social engineering,
    into performing any special functions.

For critical vulnerabilities it is recommended that you patch or upgrade as soon as possible,
unless you have other mitigating measures in place.
For example, prohibiting access to your installation from the Internet can serve as a mitigating factor.

**Severity Level: High**

Vulnerabilities that score in the high range usually have some of the following characteristics:

  * The vulnerability is difficult to exploit.
  * Exploitation could result in elevated privileges.
  * Exploitation could result in a significant data loss or downtime.

**Severity Level: Medium**

Vulnerabilities that score in the medium range usually have some of the following characteristics:

  * Vulnerabilities that require the attacker to manipulate individual victims
    via social engineering tactics.
  * Denial of service vulnerabilities that are difficult to set up.
  * Exploits that require an attacker to reside on the same local network as the victim.
  * Vulnerabilities where exploitation provides only very limited access.
  * Vulnerabilities that require user privileges for successful exploitation.

**Severity Level: Low**

Vulnerabilities in the low range typically have very little impact on an organization's business.
Exploitation of such vulnerabilities usually requires local or physical system access.
Vulnerabilities in third party code that are unreachable from |ne| code may be downgraded
to low severity.

Operating System
~~~~~~~~~~~~~~~~

In NetEye we rely on the Red Hat Enterprise Linux 8 (RHEL 8) as an OS.
This means that in terms of `RedHat Lifecycle policy <https://access.redhat.com/support/policy/updates/errata>`__ all OS packages will receive
security updates and bug fixes.

Open Source Stack
~~~~~~~~~~~~~~~~~

There are a number of third-party software integrated within NetEye, which function
in accordance with their own licenses and bug-fixing policies.

We actively cooperate with all third parties in order to fix vulnerabilities
for the convenience of our customers. On top of that, |witit| is always aiming
to detect vulnerabilities and report them to the software producers in order for them
to process according to their own security policies.
