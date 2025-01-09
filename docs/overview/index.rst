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
* Test the image

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

Here is a typical workflow:

.. image:: /images/workflow.png
   :alt: Typical workflow

Setup Launchpad
---------------

Please, follow in sequence these chapters:

* :doc:`../howto/launchpad/create_user`
* :doc:`../howto/launchpad/create_team`
* :doc:`../howto/launchpad/create_ppa`

Build packages
--------------

This process is described in:

* :doc:`../howto/packaging/package`
* :doc:`../howto/launchpad/upload_ppa`
* :doc:`../howto/launchpad/consume_ppa`
* :doc:`../howto/packaging/repackage`

Build the image
---------------

The image creation process is described in:

* :doc:`../tutorial/create_image`
* :doc:`../howto/images/create_image`

Test the image
--------------

A checklist for images is available at:

* :doc:`../reference/image_checklist`
