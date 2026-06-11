
After enabling the Sprint Releases feature, the upgrade procedure is the same as the standard upgrade procedure.
The only difference is that the ``neteye upgrade`` command will consider the available Sprint Releases when
choosing the version to upgrade to.

.. |link-pre| raw:: html

    <a href="/

.. |single-node-upgrade| raw:: html

    /update-upgrade/upgrade/upgrade.html">Single Node</a>

.. |cluster-upgrade| raw:: html

    /update-upgrade/upgrade/cluster-upgrade.html">Cluster</a>

If you already enabled the Sprint Releases feature and configured the RPM mirror repository accordingly, you can follow
the traditional procedure for |link-pre|\ |neteye_following_version|\ |single-node-upgrade| or
|link-pre|\ |neteye_following_version|\ |cluster-upgrade| upgrades.

.. note::

    When managing :term:`Satellite` nodes, remember to consider the following before proceeding with the upgrade:

    - To generate the Satellite configuration for Satellite version |neteye_version|, the NetEye Master must be
      first updated to the latest SR of |neteye_version| (usually |neteye_version|-sr4): This allows the Satellite
      configuration to be generated correctly for the minor version of the Satellite nodes.

    - When upgrading the |ne| :term:`Master` to the next minor version, remember to upgrade the connected Satellites to
      the same minor version as the |ne| :term:`Master` before upgrading the :term:`Master` to the next minor version.
      Especially when using Sprint Releases, that are not available for Satellite nodes, to avoid compatibility issues
      it is important to keep the Satellite nodes on the same minor version as the |ne| :term:`Master` before upgrading
      it to a Sprint Release of the next minor version.
