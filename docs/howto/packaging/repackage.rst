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

* A script like the following can help you to create the necessary install
  files.

  .. code:: bash

      #!/bin/sh
      #
      # Assuming that you have deb packages in directory debs/ this script will
      # determine the list of files contained in each package and create
      # *.install files for each package in a new directory tmp_install/.
      # Prior content of tmp_install/ will be deleted.

      create_install() {
        name=$1
        file=$2
        dir=$(mktemp -d)

        echo "Processing $name"

        dpkg -x $file $dir
        find $dir -type f -o -type l | sed -e "s|$dir/||" \
        | grep -v '/doc/' | grep -v '/bug/' \
        > "tmp_install/${name}.install"
        rm -rf $dir
      }

      dir="tmp_install"

      rm -rf tmp_install
      mkdir tmp_install

      find debs -name '*.deb' | while read -r file; do
        name=$(echo $file | sed -e 's|.*\/\([^\_]*\)\_.*|\1|')
        create_install $name $file
      done

      echo "Install files created in tmp_install"

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
