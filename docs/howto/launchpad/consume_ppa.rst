.. SPDX-License-Identifier: CC-BY-SA-4.0

Consuming public and private PPAs
=================================

PPAs expose packages to clients. To see the packages available in a PPA, navigate to
https://launchpad.net/~<team or username>/+archive/ubuntu/<ppa name> and look in "Overview of
published packages".

The process of consuming a PPA depends on whether the PPA is public or private.

Consuming public PPAs
----------------------

To consume a public PPA, add it to your system with the following commands:

  .. code:: bash

    sudo add-apt-repository ppa:<team or username>/<ppa name>
    sudo apt update

After adding the PPA and updating the package list, you can install the desired package using
`apt`:

  .. code:: bash

    sudo apt install <package_name>

Consuming private PPAs
-----------------------

To consume a private PPA, access must be granted by the PPA owner. To check a private PPA:

* Go to the launchpad user page: launchpad.net/~<username>
* Go to "View your private PPA subscriptions" in the PPA section
* Click "View" in the PPA you wish to consume
* Copy the source lines into a file
  ``/etc/apt/sources.list.d/<your-private-ppa>.list``
* Update the sources:

  .. code:: bash

    sudo apt update

* ``apt`` should complain that the public key used to sign the packages in the private PPA is
  missing from the system with a message similar to:

  .. code:: bash

    The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 0123456789012345

  To resolve this, the PPA's public key needs to be imported:

  .. code:: bash

    gpg --keyserver keyserver.ubuntu.com --recv-keys 0123456789012345
    gpg --export 0123456789012345 > /etc/apt/trusted.gpg.d/<your-private-ppa>.gpg
    sudo apt update
* The packages from the private PPA should now be available via ``apt``:

  .. code:: bash

    sudo apt install <package_name>

