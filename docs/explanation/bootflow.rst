.. SPDX-License-Identifier: CC-BY-SA-4.0

Boot flow
=========

This chapter provides an overview of the boot flow of RISC-V boards running
Ubuntu.

Ubuntu is booted according the UEFI standard. This requires a UEFI enabled
firmware. This could be U-Boot or EDK II.

An implementation of the Supervisor Binary Interface (SBI) running in M-mode is
required. The SBI is used for tasks like

* bring up of secondary cores
* reboot and power off

Typically OpenSBI is used as SBI implementation.

The following describes a typical boot flow based on U-Boot.

* After powering on the boot ROM of the SoC loads the software binary of the
  next boot stage from a storage device. This could be SPI-flash, eMMC or an
  SD-card. This binary may consist of a proprietary RAM initialization code and
  U-Boot SPL or RAM initialization may be integrated into U-Boot SPL.
* After RAM initialization U-Boot SPL loads a file in a format called FIT
  consisting of main U-Boot, OpenSBI, and optionally a device-tree. It then
  jumps into OpenSBI.
* OpenSBI uses the device-tree for initialization and jumps into main U-Boot
  passing the address of the device-tree.
* U-Boot uses the UEFI variables BootNext, BootOrder, and Boot#### to determine
  which binary to boot. If no match is found, the binary
  \\EFI\\BOOT\\BOOTRISCV64.EFI on the ESP is chosen. On Ubuntu for RISC-V this
  binary is GRUB.
* If the boot option does not specify a device-tree, U-Boot uses the value of
  its environment variable $fdtfile to search for a device-tree on the ESP in
  directory /dtb and loads it if present.
* U-Boot installs the loaded device-tree or if not present its internal
  device-tree as EFI configuration table. It then hands of to the loaded
  EFI binary.
* GRUB executes the script stored in /boot/grub/grub.cfg. This file may contain
  a ``devicetree`` command to load a device-tree matching the chosen Linux
  kernel.
* GRUB implements an EFI Load File 2 protocol to make the initial RAM disk
  (initrd) available to the booted kernel and installs the device-tree
  specified by the ``devicetree`` command as EFI configuration table before
  starting the Linux kernel.
* The Linux EFI stub loads the initial RAM disk using the provided
  EFI Load File 2 protocol and the device-tree from the EFI configuration table
  before entering the main kernel code.

.. image:: /images/bootflow.svg
   :target: /_images/bootflow.svg
   :alt: RISC-V Boot Flow

Device-trees
------------

For achieving best compatibility Ubuntu tries to load the device-tree that comes
with the booted kernel and matches the board.

On the Ubuntu images we have installed all RISC-V device-trees coming with the
kernel on the ESP. In most cases there is no device-tree command in grub.cfg
script at first boot.

When the Linux kernel is updated the script flash-kernel looks up the value of
the /model property of the current device-tree to determine the name of the
device-tree file that matches the hardware. It then copies the device-tree to
/boot/dtbs/<kernel-version>/<vendor>/<filename.dtb> and creates a symbolic
link /boot/dtb-<kernel-version>. Next grub-update is running. Script
/etc/grub.d/10_linux adds a ``devicetree`` command to the kernel entry if it
finds the file /boot/dtb-<kernel-version>.

.. image:: /images/kernel_update.svg
   :target: /_images/kernel_update.svg
   :alt: RISC-V Kernel Update
