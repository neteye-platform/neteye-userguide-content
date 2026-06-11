
To enable/disable sprint releases, navigate to **Configuration > Modules > update > Configuration** and enable/disable the switch
**Enable Sprint Releases**.
By enabling the Sprint Releases feature, the next ``neteye upgrade`` command will update |ne| to the latest Sprint
Release available available on the installed |ne| version.

.. note::

    It is recommended to enable the Sprint Releases feature when the |ne| version is up-to-date with the latest
    available minor version.

RPM Mirror Repository
~~~~~~~~~~~~~~~~~~~~~

If a local RPM mirror repository is in use, it is mandatory to update the mirror repository configuration to include the
Sprint Release repositories before proceeding with the upgrade of |ne|.

To correctly configure the mirror to include Sprint Releases, follow :ref:`rpm-repository-mirror-setup` setting the
``mirror_last_n_sprint_releases`` parameter to a value higher than 0, depending on the specific needs.
