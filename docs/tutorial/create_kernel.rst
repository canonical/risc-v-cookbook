.. SPDX-License-Identifier: CC-BY-SA-4.0

Your first Kernel package
=========================

This tutorial will guide you through packaging your own custom Linux kernel 
using `ukpack`.

Requirements
------------

You need a Git repository containing your custom kernel tree.  
For this tutorial, we will use the upstream Linux kernel and modify it 
to simulate a custom kernel.

Install Dependencies
--------------------

Ensure the required packages are installed:

.. prompt:: bash $ auto

    $ sudo apt-get update
    $ sudo apt-get install git wget devscripts

Clone Required Repositories
---------------------------

First, clone the `ukpack` repository:

.. prompt:: bash $ auto

    $ git clone https://kernel.ubuntu.com/forgejo/esmil/ukpack.git

Next, download the upstream Linux kernel source on which the custom kernel is
based, in our case this is version ``6.6.20``:

.. prompt:: bash $ auto

    $ wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.20.tar.xz

Create a Custom Kernel Repository
---------------------------------

We will now create a new repository for a custom kernel based on the 
official upstream Linux kernel ``6.6.20``.

.. prompt:: bash $ auto

    $ git clone --depth 1 --branch v6.6.20 \
      https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git \
      custom-linux
    $ cd custom-linux/

Make an Example Change to the Kernel
------------------------------------

To simulate a custom kernel for this tutorial, here we modify the `EXTRAVERSION`
field in the `Makefile`. Here is where you should apply your changes.

.. prompt:: bash $ auto

    $ sed -i 's/^EXTRAVERSION =$/EXTRAVERSION = -custom1/' Makefile

Now, commit the change:

.. prompt:: bash $ auto

    $ git add Makefile
    $ git commit -m "Add custom EXTRAVERSION"

Sync with Upstream Kernel
-------------------------

Get the upstream Linux kernel tag corresponding to the version the custom kernel
is based on:

.. prompt:: bash $ auto

    $ git remote add linux-stable \
      https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
    $ git fetch --depth 1 linux-stable tag v6.6.20

This tag will be used to produce a diff between the custom kernel tree and the
upstream one.

Define the Kernel Package Configuration
---------------------------------------

Inside the `custom-linux` directory, create a `custom_kernel.toml` file:

.. code:: text

    custom-kernel (6.6.20-1.1ubuntu1) noble; urgency=medium
     * Initial packaging
     -- Your Name <you_email@example.com> Wed, 12 Mar 2025 14:02:38 +0100
    ---
    arch = "riscv64"
    config = "arch/riscv/configs/defconfig"
    orig = "v6.6.20"

    [pkg.source]
    Maintainer = "your_email@example.com"

Build the Kernel Package
------------------------

Create an output directory and run `ukpack`:

.. prompt:: bash $ auto

    $ mkdir ../ukpack.output/
    $ ../ukpack/ukpack -o ../linux-6.6.20.tar.xz -t . \
      -d ../ukpack.output/ custom_kernel.toml

Sign the Package
----------------

Move to the output directory and sign the package:

.. prompt:: bash $ auto

    $ cd ../ukpack.output
    $ debsign *.changes

Next Steps
----------

After signing, you can install or upload the package for further testing.
