.. SPDX-License-Identifier: CC-BY-SA-4.0

Overview
========

The RISC-V Image Cookbook is meant to help users to spin Ubuntu based images
for hardware that is not yet supported by the Ubuntu distribution.

It will guide you through these steps of your project:

* Collect requirements
* Setup Launchpad
* Build packages
* Build the image

.. note::

   **This documentation is still in draft status.**

Collect requirements
--------------------

For getting started you will have to create an overview of which aspects
of current Ubuntu does not fulfill your needs. Typical items are:

* vendor Linux kernel
* vendor Xorg and Mesa packages
* vendor U-Boot required on the image
* additional packages
* configuration files

Next identify the licenses and other legal requirements like non-disclosure
agreements of the extra required packages.

If source code must not be disclosed to the public,
private personal package archives (PPAs) on Launchpad can be used.

Setup Launchpad
---------------

Build packages
--------------

Build the image
---------------

