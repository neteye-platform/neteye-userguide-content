
All the |ne| 4 ISO images can be found on the `NetEye download site
<https://repo.wuerth-phoenix.com/neteye-iso/>`_.  To be sure you have
downloaded a valid image, the following verification procedure must be
followed.

Import the public GPG key
~~~~~~~~~~~~~~~~~~~~~~~~~

Go to the `Downloads section
<https://www.neteye-blog.com/downloads/>`_ of the |ne| blog, then move
to `NetEye --> GPG public key` and save **public-gpg-key**. From the
CLI, extract the archive and then import the key with the following
command::

  # gpg --import public.gpg

Verify now the imported key::

  # gpg --fingerprint net.support@wuerth-phoenix.com

If the fingerprint matches the one reported on the |ne| blog, you have
the right key installed on your system.

Download and Verify the ISO
~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| is shipped as ISO images and a link to the Würth IT Italy
repository containing the images and supporting material will be
provided by the Support Team after the contract is signed.

From the link provided you will need to download the following files.

- The desired ISO file (always download the most recent one)
- The :file:`sha256sum.txt.asc` file

Save both files in the same directory, then proceed to check that they
are not corrupted: from the CLI, move to the directory where the files
have been saved, then execute the following commands.

To verify the :file:`sha256sum.txt.asc` file execute::

  # gpg --verify sha256sum.txt.asc

The output will look something like this:

.. container:: codeblock

    .. code::

       gpg: Signature made Tue 29 Sep 2020 03:50:01 PM CEST
       gpg:               using RSA key B6777D151A0C0C60
       gpg: Good signature from "Wuerth Phoenix <net.support@wuerth-phoenix.com>" [unknown]
       gpg: WARNING: This key is not certified with a trusted signature!
       gpg:          There is no indication that the signature belongs to the owner.
       Primary key fingerprint: E610 174A 971E 5643 BC89  A4C2 B677 7D15 1A0C 0C60

Once you have verified the signature of the :file:`sha256sum.txt.asc`
file, you can then verify the ISO file with the following command::

   # sha256sum -c sha256sum.txt.asc 2>&1 | grep OK

The output will look something like this:

.. container:: codeblock

    .. code::

       # ./neteye4.23-rhel8.stable.iso: OK

At this point the ISO file is verified and it is ready to be used.  In
case the output is different from **OK**, the ISO image may be
corrupted and needs to be downloaded once more.

.. note:: Both ISO and :file:`sha256sum.txt.asc` files are generated
   nightly, so make sure to always download both of them each time you need.

.. seealso:: More details about GPG and the verification process can
   be found on our blog:

   https://www.neteye-blog.com/2020/12/how-to-verify-the-integrity-of-your-neteye-4-iso-image/
