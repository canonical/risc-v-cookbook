.. SPDX-License-Identifier: CC-BY-SA-4.0

Upload to a PPA
===============

PPAs accept source code for Debian packages. A PPA will then build the package
from these sources and make the resulting binaries available for consumption,
as explained in :doc:`Consuming public and private PPAs <consume_ppa>`. This
guide explains how to upload a Debian source to an existing PPA. To create a PPA
please refer to :doc:`Create PPAs <create_ppa>`

To upload to an existing PPA:

* For uploading via sftp your SSH key must have been provided to Launchpad
  as described in :doc:`Create Launchpad user <create_user>`.
* Install the upload tool dput

  .. prompt:: bash $ auto

    sudo apt-get install dput

* Navigate to your Debian package source directory. This is the directory where the
  debian/ folder is.
* Ensure that the Debian version is correctly set up. See :ref:`Debian version numbering`.
* If necessary, create a source archive (orig.tar.xz).
  See :ref:`create_source_archive`.
* Build the package:

  .. code:: bash

    dpkg-buildpackage -S -sa -d

  this will generate the necessary files in the parent directory.
* Navigate to the parent directory and upload to the PPA:

  .. code:: bash

    cd ..
    dput ppa:<team or username>/<ppa_name> <source.changes>

  where <source.changes> is the .changes file that was just created in this parent
  directory. To upload to a team PPA it is necessary to be a member of the team. The
  PPA can also be added by writing the following to ~/.dput.cf:

  .. code:: bash

    [my-ppa]
    fqdn = ppa.launchpad.net
    method = sftp
    incoming = ~<team or username>/ubuntu/<ppa_name>/
    login = <username>

  where "my-ppa" is the name desired to refer to the PPA (but do not use "ppa"). An
  upload to the PPA can then be issued with

  .. code:: bash

    dput my-ppa <source.changes>

.. _Debian version numbering:

Debian version numbering
------------------------

The current version of the Debian sources is set in the first line of ``debian/changelog``.
For the PPA, we should change the version in the changelog to one that's lower than the
version we plan to release. For example:

  .. code-block:: none

    -postfix (3.3.0-1ubuntu0.1) bionic; urgency=medium
    +postfix (3.3.0-1ubuntu0.1~bionic1) bionic; urgency=medium

Since the tilde ~ character sorts lower than everything else in Launchpad, we can append
``~<string>1`` to the version string in ``debian/changelog``. Having a numeric digit in
this suffix is important because once Launchpad has accepted your upload, it won't accept
another one with the same version number (nor any earlier version number). So if you need
to fix something in your upload -- even just copyediting your changelog entry -- you need
an incrementally higher version number. Incrementing this suffix in the debian revision
allows you to do this without needing to modify the upstream version number.

The current version of the Debian sources in the first line of ``debian/changelog`` follows
the syntax

  .. code-block:: none

    package_name (version) distribution(s); urgency=urgency

where ``version`` is structured as

  .. code-block:: none

    [epoch:]upstream-version[-debian-revision]

- **epoch** (optional): A non-negative integer followed by ``:``. Used to override older
  versioning schemes. Example: ``1:2.0.1``.

- **upstream-version**: The version from upstream. Can include alphanumeric characters,
  periods (``.``), and plus signs (``+``), but not hyphens (``-``). Example: ``2.0.1+dfsg``.

- **debian-revision** (optional): Starts at ``1`` for the initial Debian packaging and
  increments with changes to Debian-specific files. Example: ``2ubuntu1``.

If the Debian source has no upstream counterpart, it is called a *native* package. Native
packages do not include a ``debian-revision`` in their version field. If there is an upstream
source, which is often the case, the package is called *non-native*, and the version includes
a ``debian-revision``. Changes that only increase the ``debian-revision`` must affect only the
``debian/`` folder. Examples include updating patches in ``debian/patches`` or modifying
packaging scripts.

.. _create_source_archive:

Create source archive
---------------------

When uploading to a PPA, it is necessary to provide an archive containing the
source code that needs to be built.

For **native Debian packages** (i.e., packages without the ``debian-revision`` field in
the version number, as explained in the "Debian version numbering" section), running
the ``dpkg-buildpackage -S -sa -d`` command will already generate a
``<package_name>_<version>.tar.gz`` file in the parent directory of the Debian sources.

For **non-native Debian packages** (i.e., those with a ``debian-revision`` field in the
version number), the ``dpkg-buildpackage`` command will create a
``<package_name>_<upstream-version>.debian.tar.xz`` file containing the contents of the
``debian/`` folder, but it expects an already existing file
``<package_name>_<upstream-version>.orig.tar.{bz2,gz,lzma,xz}`` in the parent directory
of the Debian sources, containing the Debian sources with the ``debian/`` folder excluded.
If not present, this can be created from the Debian source directory:

  .. code-block:: bash

    tar -cJf ../<package_name>_<upstream-version>.orig.tar.xz --exclude=debian .
