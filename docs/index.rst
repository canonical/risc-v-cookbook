.. SPDX-License-Identifier: CC-BY-SA-4.0

RISC-V Image Cookbook
=====================

The RISC-V Image Cookbook is meant to help users to spin Ubuntu based images
for hardware that is not yet supported by the Ubuntu distribution.

It will guide you through these steps of your project:

* Collect requirements
* Setup Launchpad
* Build packages
* Build the image

Collect requirements
--------------------

For getting started you will have to create an overview of which aspects
of current Ubuntu does not fullfill your needs. Typical items are:

* vendor Linux kernel
* vendor Xorg and Mesa packages
* vendor U-Boot required on the image
* additional packages
* configuration files

Next identify the licenses and other legal requirements like non-disclosure
aggreements of the extra required packages.

If source code must not be disclosed to the public,
private personal package archives (PPAs) on Launchpad can be used.

Setup Launchpad
---------------

Build packages
--------------

Build the image
---------------


Contributing
------------

The RISC-V Image Cookbook is community effort and welcomes community projects,
contributions, suggestions, fixes and constructive feedback.

To contribute create an issue or merge request on
https://github.com/canonical/risc-v-cookbook.

-----

:h2:`In this documentation`

.. toctree::
   :hidden:
   :maxdepth: 2

   tutorial/index
   howto/index
   reference/index
   explanation/index

   contributing


.. grid:: 1 1 2 2

   .. grid-item-card:: :doc:`Tutorial <tutorial/index>`
      :link: tutorial/index
      :link-type: doc

      **Start here**: a hands-on introduction to Example Product for new users

   .. grid-item-card:: :doc:`How-to guides <howto/index>`
      :link: howto/index
      :link-type: doc

      **Step-by-step guides** covering key operations and common tasks

.. grid:: 1 1 2 2
   :reverse:

   .. grid-item-card:: :doc:`Reference <reference/index>`
      :link: reference/index
      :link-type: doc

      **Technical information** - specifications, APIs, architecture

   .. grid-item-card:: :doc:`Explanation <explanation/index>`
      :link: explanation/index
      :link-type: doc

      **Discussion and clarification** of key topics
