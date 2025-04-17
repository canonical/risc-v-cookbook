.. SPDX-License-Identifier: CC-BY-SA-4.0

Your first kernel package
=========================

This tutorial guides you through packaging your own custom Linux kernel
using ``ukpack``.

Requirements
------------

You need a Git repository containing your custom kernel tree.  
For this tutorial, we use the upstream Linux kernel and modify it
to simulate a custom kernel.

Install dependencies
--------------------

Ensure the required packages are installed:

.. prompt:: none $ auto

    $ sudo apt-get update
    $ sudo apt-get install git wget devscripts

Clone required repositories
---------------------------

Clone the ``ukpack`` repository:

.. prompt:: none $ auto

    $ git clone https://kernel.ubuntu.com/forgejo/esmil/ukpack.git

Create a custom kernel repository
---------------------------------

Now create a new repository for a custom kernel based on the
official upstream Linux kernel, ``6.6.20``.

.. prompt:: none $ auto

    $ git clone --depth 1 --branch v6.6.20 \
      https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git \
      custom-linux
    $ cd custom-linux/

Make an example change to the kernel
------------------------------------

To simulate a custom kernel for this tutorial, modify the ``EXTRAVERSION``
field in the ``Makefile``. Here is where you apply your changes.

.. prompt:: none $ auto

    $ sed -i 's/^EXTRAVERSION =$/EXTRAVERSION = -custom1/' Makefile

Now, commit the change:

.. prompt:: none $ auto

    $ git add Makefile
    $ git commit -m "Add custom EXTRAVERSION"

Sync with upstream kernel
-------------------------

Get the upstream Linux kernel tag corresponding to the version the custom kernel
is based on:

.. prompt:: none $ auto

    $ git remote add linux-stable \
      https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
    $ git fetch --no-tags linux-stable tag v6.6.20

This tag is used to produce a diff between the custom kernel tree and the
upstream one. That the tag is correctly fetched can be checked with ``git tag | grep v6.6.20``.

Define the kernel package configuration
---------------------------------------

Inside the ``custom-linux`` directory, create a ``custom_kernel.toml`` file:

.. code:: text

    linux-tutorial (6.6.20-1.1ubuntu1) noble; urgency=medium

     * Initial packaging

     -- Your Name <you_email@example.com> Wed, 12 Mar 2025 14:02:38 +0100
    ---
    arch = "riscv64"
    # Use the architecture's upstream defconfig
    config = "defconfig"
    orig = "v6.6.20"

    [pkg.source]
    Maintainer = "your_email@example.com"

The name of the kernel package should always be of the form ``linux-<custom flavour>``, and the
version should be of the form ``<kernel_version>-<local_version>``, where ``<kernel_version>`` is
the Linux kernel source on which the custom kernel is currently based. Also, this tutorial uses
the `noble` Ubuntu release.

Build the kernel package
------------------------

Download the Linux kernel source on which the custom kernel is currently based,
which in this case is Linux version ``6.6.20``, create an output directory
outside your kernel tree, and run ``ukpack``:

.. prompt:: none $ auto

    $ wget -P .. https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.20.tar.xz
    $ mkdir ../ukpack.output/
    $ ../ukpack/ukpack -o ../linux-6.6.20.tar.xz \
      -d ../ukpack.output/ custom_kernel.toml

Sign the package
----------------

Move to the output directory and sign the package:

.. prompt:: none $ auto

    $ cd ../ukpack.output
    $ debsign *.changes

Next steps
----------

After signing, you can install or upload the package for further testing.
