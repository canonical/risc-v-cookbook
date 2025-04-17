Repackaging binaries
====================

Sometimes it is not allowable to publish the source code for vendor code
packages. In this case, Debian packages can be built in a private PPA.
The artifacts can be repackaged without source and published in a public
PPA afterwards.

The following steps are needed.


Download Debian packages
------------------------

* In a browser, navigate to the private PPA containing the Debian packages.
* Download all architecture independent ``\*_all.deb`` files.
* Download all ``\*.deb`` files for the relevant architectures.

Create source of binary package
-------------------------------

* Extract all debian Packages to a directory matching the source package name
  and the version:

  .. code:: none

      find . -name '*.deb' -exec dpkg -x {} xorg-server-21.1.13 \;

* Unzip all man pages:

  .. code:: none

      find xorg-server-21.1.13/usr/share/man/ -type f -name '*.gz' \
        -exec gzip -d {} \;

* Fix all symbolic man-page links.

  You can find them with:

  .. code:: none

      find xorg-server-21.1.13/usr/share/man/ -type l -name '*.gz'

  Delete the existing link files and replace them with ones without
  .gz-ending.

* Create the ``\*.orig.tar.xz`` file.

  .. code:: none

      tar -cJf xorg-server_21.1.13.orig.tar.xz xorg-server-21.1.13/

* Copy the debian packaging from the source package

  .. code:: none

      cp -R /path/to/source/package/xorg-server-21.1.13/debian \
            xorg-server-21.1.13/

Edit Debian packaging
---------------------

* Remove the ``debian/patches/`` directory.

* In ``debian/control``, remove all build dependencies (``Build-Depends:``,
  ``Build-Depends-Indep:``) but :pkg:`debhelper-compat`.

* Change ``debian/source/format`` to:

  .. code:: text

      3.0 (quilt)

* Change ``debian/rules`` to:

  .. code:: text

      #!/usr/bin/make -f

      %:
              dh $@

* Change the ``debian/docs`` and ``debian/\*.docs`` files to point to the correct source
  paths in ``usr/share/docs``.

Validate the Debian packaging
-----------------------------

* Submit the package to a different PPA to avoid version-numbering collisions.

* Check for build failure and fix these.

* Check for missing files in the Debian packages and add these to
  ``/debian/\*.install``.
