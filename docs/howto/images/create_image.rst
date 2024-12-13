.. SPDX-License-Identifier: CC-BY-SA-4.0

Create Image With ubuntu-image
==============================

For creating an image with ubuntu-image we need

* a git repository with the gadget definition
* a image definition file image-definition.yaml which may be located in the same
  git repository

Install ubuntu-image
--------------------

ubuntu-image is provided as a snap and can be installed with

.. prompt:: bash $ auto

    $ sudo snap install ubuntu-image

Gadget definition
-----------------

The gadget definition is provided as a Yaml file meta/gadget.yaml.
It describes

* disk partitions of the image
* files installed on partitions other than the root partition

