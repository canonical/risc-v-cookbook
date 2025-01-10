.. SPDX-License-Identifier: CC-BY-SA-4.0

Tools
=====

This is a collection of useful tools.

Ensure that `ubuntu-dev-tools` is installed

.. code:: bash

    sudo apt install ubuntu-dev-tools

* `dpkg-buildpackage <https://man7.org/linux/man-pages/man1/dpkg-buildpackage.1.html>`_ - Build binary or source packages from Debian source files.

    * Build an unsigned package from the Debian source directory:

      .. code:: bash

          dpkg-buildpackage -us -uc

* `sbuild <https://wiki.debian.org/sbuild>`_ - A tool for building packages in a chroot or container from Debian source files or `.dsc` files.

    * Build a package from the Debian source directory:

      .. code:: bash

          sbuild

    * Build from a `.dsc` file:

      .. code:: bash

        sbuild package_*.dsc

* `pull-lp-source <https://manpages.ubuntu.com/manpages/xenial/man1/pull-lp-source.1.html>`_ - Download a binary or source package from Launchpad.

    * Download a package for the Plucky Puffin release:

      .. code:: bash

          pull-lp-source u-boot-starfive plucky

* `pull-ppa-source <https://manpages.debian.org/unstable/ubuntu-dev-tools/pull-ppa-source.1.en.html>`_ - Download a source package from a PPA.

    * Download a source package from a PPA:

      .. code:: bash

          pull-ppa-source --ppa user/ppa-name package_name
