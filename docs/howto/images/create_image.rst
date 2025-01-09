.. SPDX-License-Identifier: CC-BY-SA-4.0

Create customized image with ubuntu-image
=========================================

For creating an image with ubuntu-image we need

* a git repository with the gadget definition
* a image definition file image-definition.yaml which may be located in the same
  git repository

Please, execute the :doc:`/tutorial/create_image` tutorial.
This provides a sample project.

Install dependencies
--------------------

.. prompt:: text $ auto

    $ sudo apt-get update
    $ sudo apt-get install git snapd qemu-user-static ubuntu-dev-tools
    $ sudo systemctl restart systemd-binfmt

Install ubuntu-image
--------------------

ubuntu-image is provided as a snap and can be installed with

.. prompt:: bash $ auto

    $ sudo snap install --classic ubuntu-image

Create a gadget definition
--------------------------

At a minimum the Gadget definition consists of

* the definition file gadget.yaml
* a Makefile where the default target
  * copies gadget.yaml to directory ``meta/``
  * copies all installation files to directory ``install/``

The YAML file format is described in https://yaml.org/spec/.

An overview of the available fields in gadget.yaml is provided in
:doc:`/reference/gadget_definition/`.

The gadget definition is provided as a Yaml file meta/gadget.yaml.
gadget.yaml describes

* partitions of the image
* files installed on partitions other than the root partition

