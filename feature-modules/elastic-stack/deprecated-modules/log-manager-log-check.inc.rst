Log check
---------

.. _log-checks:

Log Integrity Checks
~~~~~~~~~~~~~~~~~~~~

The “Log Check” section provides a list of all log files with the
corresponding validation result. The Log Check performs a real-time
validation to verify:

- Whether the log date and date of last modification on the system are
  equal: This ensures that the log file has not been altered by
  anyone.
- If the signature file exists and that the signature checksum
  recalculation returns the same value as the content of the log file:
  The hash from the GPG key guarantees that the file has not been
  changed after signature calculation.
- That the timestamp of the signature corresponds to the date after
  the day when the log files have been collected. This is because
  during the date of logging, the log file is continuously written to
  and the final signature can only be created and stored once this
  process terminates.
- That the integrity of the entire folder is valid. An MD5 checksum is
  calculated for the whole folder of a day and stored in the archive
  of the next day. The hash of the next day's checksum is calculated
  together with the checksum of the previous day. ANY modification on
  the SHA-512 file will lead to corruption of the hash result for that
  next day, and the same will occur for ANY modification of any log
  file and their signatures.

The log check system in NetEye 4 assumes that the logs collected by
rsyslog are organized in a folder with the following logical structure::

  host_name/year/month/day/

At midnight, a script signs the just-completed day's folder following
these steps:

- Compression of all log files stored in that day's folder.
- Creation of a .sig signature file for each compressed file using
  GPG.
- Creation of the file "YYYYMMDD.hashes" which is saved in the folder
  for the next day, using the same path format as above. Inside this
  file, the hashes of all the files' contents from the just-finished
  day are stored in the form "[hash] [full-path]". for Example::

    ABACC417349AE2.....0413E6FA4E89A9  /var/.../ host_name/year/month/day/log1.log.gz

There are three different systemd timers installed by default that
periodically execute these tasks:

- *icingaweb2-module-logmanager-extend-blockchain.timer* - runs
  **every day after midnight** and extends automatically the log block
  chain (see Log Manager's
  :ref:`logmanager-concept-technical-overview`).
- *icingaweb2-module-logmanager-verify-blockchain-light.timer* - runs
  **every 12 hours starting from 01:00** and carries out a light check of the logs
  blockchain.
- *icingaweb2-module-logmanager-verify-blockchain-deep.timer* runs
  **every 24 hours** and performs a deep check of the logs blockchain.

Usage of Log Check
~~~~~~~~~~~~~~~~~~

When you click on the "Log Check" action in the Log Manager dashboard,
the "Light Check" and "Deep Check" panels will appear to the right
(:numref:`figure-logcheck`). The signature check is performed by
selecting a single host to test, or on all hosts by selecting the "All
hosts" option in the dropdown menu.

.. _figure-logcheck:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logcheck-01.png
   :alt: Integrity checks

   Integrity checks with the Light and Deep check options.

When the check is complete, a check symbol will appear in the top-right
corner of the page: if all requested checks are correct, then a green
symbol is shown. Otherwise if at least one of these checks fails, then a
red symbol will appear.

.. rubric:: Light Check Option

The light check examines all log files verifying that the signature file
exists and is not corrupted.

.. _figure-logcheck-light:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logcheck-02.png
   :alt: The Light check panel

   The Light check panel.

.. rubric:: Deep Check Option

The deep check verifies logs using the following logic: - It first
conducts a light check on the logs as described above. - It verifies
that the date of last modification (established by the file system) of
each file contained in the just-finished day is not later than midnight
(plus a small tolerance threshold) of the following day. - It checks for
each hash/path pair in the YYYYMMDD.hashes file in the following day's
directory that the hash corresponds exactly to a recomputed hash of the
file at the indicated path, and that the number of files in the
directory has not changed.

.. _figure-logcheck-deep:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-manager-logcheck-03.png
   :alt: The Deep check panel

   The Deep check panel.

Example of errors discovered by the log check can be seen in
:numref:`figure-logcheck-deep`.
