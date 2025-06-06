.. SPDX-License-Identifier: CC-BY-SA-4.0

Package a custom kernel with ukpack
===================================

This guide explains how to create a custom Linux kernel package using the
``ukpack`` tool. To create a kernel package a Git repository containing a custom
kernel tree is needed. The repository should be based on an upstream Linux
kernel version ``<kernel_version>``.

First, execute the :doc:`/tutorial/create_kernel` tutorial.
This provides a sample project.

Clone required repositories
---------------------------

First, clone the ``ukpack`` repository:

.. prompt:: none $ auto

    $ git clone https://kernel.ubuntu.com/forgejo/esmil/ukpack.git

Next, clone the custom kernel repository:

.. prompt:: none $ auto

    $ git clone https://github.com/your_username/your_custom_kernel.git
    $ cd your_custom_kernel/

Sync with upstream kernel
-------------------------

If the custom kernel tree is based on the Linux stable branch, you can add the upstream repository
as a remote and fetch a specific tag:

.. prompt:: none $ auto

    $ git remote add linux-stable https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
    $ git fetch --no-tags linux-stable tag v<kernel_version>

It can be checked that the tag is there with ``git tag | grep v<kernel_version>``.

Create kernel package configuration
-----------------------------------

Inside the directory of your custom kernel repository (``your_custom_kernel``),
create a file ``my_kernel.toml`` file (e.g. using the command
``dch --create -c my_kernel.toml``) with the following content:

.. code:: text

    linux-<custom_flavour> (<kernel_version>-<local_version>) <release>; urgency=medium

     * Initial packaging

     -- Your Name <your_email@example.com> Wed, DD MMM YYYY HH:MM:SS +TZ
    ---
    arch = "<your_architecture>"
    # provide your full kernel configuration
    #config = "./my.config"
    # ..a defconfig file
    #config = "./my_defconfig"
    # ..or just use the architecture's upstream defconfig
    config = "defconfig"
    orig = "v<kernel_version>"

    [pkg.source]
    Maintainer = "your_email@example.com"

This file contains both configuration options (below the ``---``) and changelog
entries keeping track of changes to your custom kernel. At first, this file has
to be created manually, but on re-packaging, updates to this file can be done
with the ``dch -c my_kernel.toml`` command, which automatically prepends
a changelog entry on top of the last one.

Build the kernel package
------------------------

Download the Linux kernel version on which the custom kernel is currently based, create an output
directory outside your kernel tree, and run ``ukpack``:

.. prompt:: none $ auto

    $ wget -P .. https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-<kernel_version>.tar.xz
    $ mkdir ../ukpack.output/
    $ ../ukpack/ukpack -o ../linux-<kernel_version>.tar.xz -d ../ukpack.output/ my_kernel.toml

Sign the package
----------------

Change into the output directory and sign the package:

.. prompt:: none $ auto

    $ cd ../ukpack.output
    $ debsign *.changes

Next steps
----------

After signing, you can proceed with testing or uploading the package.
