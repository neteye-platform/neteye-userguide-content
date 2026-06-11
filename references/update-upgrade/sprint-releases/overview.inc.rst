

Sprint Releases (abbreviated SR) are specific versions of NetEye that are released at the end of a development Sprint.
These releases are cumulative progressions of the minor NetEye version that is currently in development and typically
include new features, improvements, and bug fixes that have been developed during the two-week sprint cycle.

Why Sprint Releases
~~~~~~~~~~~~~~~~~~~

Enabling Sprint Releases feature provides early access to new features and improvements that are not yet available in the
stable version of NetEye. This allows users to access new features and improvements before they are released in the stable
version of NetEye that is typically released every 2 months.
Sprint Releases will decrease the time to market for new features and improvements to just two weeks, which is the duration
of a development sprint cycle.

Furthermore, users are not required to keep the system on Sprint Releases permanently; they can update when
interested and disable the SR functionality once the interested feature is included in the minor release.

How it works
~~~~~~~~~~~~

When Sprint Releases feature is enabled, only the NetEye version of the :term:`Master` will be affected. While all the connected
:term:`Satellite` machines will remain on the minor version of NetEye, launching the ``neteye upgrade`` command on
the |ne| :term:`Master` will update NetEye to the latest Sprint Release available with respect to the installed |ne| version.


.. note::

    Since the intended use of Sprint Releases is to provide early access to new features and improvements, it is recommended
    to constantly update the NetEye version to the latest Sprint Release available to benefit from the latest features and
    improvements.

    For this reason, **non-final** Sprint Releases will not receive bug fixes, which instead will be available for the
    :term:`Final Sprint Release` of the latest available |ne| Minor.
    Given the short Sprint cycle, the next release with the bug fix will be available in a maximum of two weeks.

    On the other hand, **OS-related bugs** will be still fixed in the current Sprint Release and
    their fix can be applied by updating |ne|.

Upgrade Scenarios Examples
~~~~~~~~~~~~~~~~~~~~~~~~~~

The following is the classical upgrade path between |ne| Minor Releases.

..  image:: /references/update-upgrade/img/standard-scenario.svg

When Sprint Releases are enabled, the upgrade process for NetEye differs from that of NetEye Minor Releases.
This section outlines various scenarios to clarify which version of NetEye your installation will be upgraded to.

Scenario 1: Upgrade from an SR different from the final of a given Release
``````````````````````````````````````````````````````````````````````````

Suppose your |ne| is currently on SR number 2 of the NetEye 4.40 minor, referred to as ``4.40-sr2``.
After upgrading to version ``4.40-sr2``, several Sprint Releases were published, up to ``4.41-sr2`` (as illustrated in the diagram below).

..  image:: /references/update-upgrade/img/SR-scenario1.svg

When you perform a |ne| upgrade, your installation will be updated to the latest SR of your current
minor version, which is ``4.40-sr4``. This is necessary because you must always transition through the last SR
of your current minor version before moving to an SR of the next minor version (in this case, ``4.41``).

Scenario 2: Upgrade from the final SR of a given Minor Release
``````````````````````````````````````````````````````````````

Building on the previous scenario, let’s say your |ne| is now at the SR  ``4.40-sr4``.

..  image:: /references/update-upgrade/img/SR-scenario2.svg

Since you are at the final SR of the 4.40 minor version, your upgrade will take you to the latest available
SR of the next minor version, which in this case is ``4.41-sr2``.

Scenario 3: Upgrade after disabling SRs
```````````````````````````````````````

Let’s say you would like to disable Sprint Releases and return to the classical stream of |ne| Minor Releases.

In this case, based on the sprint release that is installed on the system, the two following scenarios are possible:

- If you are **on the Final Sprint Release** of a given minor version, an upgrade will allow you to upgrade |ne| to the following
  minor version, when released. Up to that moment, you will continue to receive the latest bug fixes.
  For example, if you are on ``4.40-sr4`` and you disable SRs, you will be able to upgrade to ``4.41`` when it is released.
- If you are not **on the Final Sprint Release** of a given minor version, an upgrade will bring you to the stable version within
  the same minor, to allow the installation of the latest bugfixes, as soon as it is released.
  Up to that moment, you will not receive any additional updates.
  For example, if you are on ``4.40-sr2`` and you disable SRs, you will be able to upgrade to ``4.40`` |ne| Minor Release
  when it is released.
