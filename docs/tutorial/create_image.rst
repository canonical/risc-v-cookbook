.. SPDX-License-Identifier: CC-BY-SA-4.0

Your first Ubuntu image
=======================

This tutorial will walk you through creating your first Ubuntu image
and launching it in a virtual machine.

Install dependencies
--------------------

.. prompt:: text $ auto

    $ sudo apt-get update
    $ sudo apt-get install git snapd qemu-user-static ubuntu-dev-tools
    $ sudo systemctl restart systemd-binfmt
    $ sudo snap install --classic ubuntu-image

Clone the gadget repository
---------------------------

.. prompt:: text $ auto

    $ git clone https://github.com/canonical/risc-v-gadget.git

Build the image
---------------

.. prompt:: text $ auto

    $ cd risc-v-gadget
    $ sudo ubuntu-image --workdir workdir --debug classic image-definition.yaml

Test the image
--------------

For running the image in a virtual machine install the runtime dependencies.

.. prompt:: text $ auto

    $ sudo apt-get install opensbi qemu-system-riscv64 u-boot-qemu

Navigate to the image and change the owner.

.. prompt:: text $ auto

    $ cd workdir
    $ sudo chown $USER ubuntu-*-preinstalled-server-riscv64.img

Launch the virtual machine.

.. prompt:: text $ auto

    $ qemu-system-riscv64 \
      -machine virt -nographic -m 2048 -smp 4 \
      -bios /usr/lib/riscv64-linux-gnu/opensbi/generic/fw_jump.bin \
      -kernel /usr/lib/u-boot/qemu-riscv64_smode/uboot.elf \
      -device virtio-net-device,netdev=eth0 -netdev user,id=eth0 \
      -device virtio-rng-pci \
      -drive file=ubuntu-*-preinstalled-server-riscv64.img,format=raw,if=virtio

Login with user *ubuntu* and password *ubuntu*.

You will be asked to change the password.

Power off the virtual machine with

.. prompt:: text $ auto

    $ sudo poweroff
