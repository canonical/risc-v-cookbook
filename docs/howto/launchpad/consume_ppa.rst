.. SPDX-License-Identifier: CC-BY-SA-4.0

Consuming Public and Private PPAs
=================================

PPAs expose packages to clients. To see the packages available in a PPA, navigate to
:samp:`https://launchpad.net/~{<team or username>}/+archive/ubuntu/{<ppa name>}`
and look in "Overview of published packages".

The process of consuming a PPA depends on whether the PPA is public or private.

Consuming public PPAs
----------------------

To consume a public PPA, add it to your system with the following commands:

  .. prompt:: bash $ auto

    $ sudo add-apt-repository ppa:<team or username>/<ppa name>
    $ sudo apt update

After adding the PPA and updating the package list, you can install the desired package using
`apt`:

  .. prompt:: bash $ auto

    $ sudo apt install <package_name>

Consuming private PPAs
-----------------------

To consume a private PPA, access must be granted by the PPA owner. To check a private PPA:

* Go to the Launchpad user page :samp:`https://launchpad.net/~{<username>}`
* The link "View your private PPA subscriptions" in the PPA section get you
  to :samp:`https://launchpad.net/~{username}/+archivesubscriptions`
* Click "View" in the PPA you wish to consume.
  You will see two lines like in the example below:

  .. code:: text

      deb https://johndoe:RZ5mq1R0XVlZ5ZJW102g@private-ppa.launchpadcontent.net/my-private-team/myppa/ubuntu plucky main #Personal access of John Doe (johndoe) to My PPA
      deb-src https://johndoe:RZ5mq1R0XVlZ5ZJW102g@private-ppa.launchpadcontent.net/my-private-team/myppa/ubuntu plucky main #Personal access of John Doe (johndoe) to My PPA

  In the example

  * ``deb`` refers to binary packages
  * ``deb-src`` refers to package sources
  * ``johndoe`` is the user name
  * ``RZ5mq1R0XVlZ5ZJW102`` is the password
  * ``my-private-team`` is the name of a team
  * ``myppa`` is the name of the PPA
  * ``plucky`` is an Ubuntu release
  * ``main`` is the component of the Ubuntu repository

* Copy the lines into a file
  :samp::`/etc/apt/sources.list.d/{team-name}-{ppa-name}>.list`
* Adjust the Ubuntu release as needed.
* Update the sources:

  .. prompt:: bash $ auto

    $ sudo apt update

* ``apt`` may complain that the public key used to sign the packages in the
  private PPA is missing from the system with a message similar to:

  .. code:: text

    The following signatures couldn't be verified because
    the public key is not available: NO_PUBKEY 0123456789012345

  To resolve this, the PPA's public key needs to be imported:

  .. prompt:: bash $ auto

    $ gpg --keyserver keyserver.ubuntu.com --recv-keys 0123456789012345
    $ gpg --export 0123456789012345 > /etc/apt/trusted.gpg.d/<your-private-ppa>.gpg
    $ sudo apt-get update

* The packages from the private PPA should now be available via ``apt``:

  .. prompt:: bash $ auto

    $ sudo apt install <package_name>
