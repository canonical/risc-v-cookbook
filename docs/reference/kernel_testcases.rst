.. SPDX-License-Identifier: CC-BY-SA-4.0

Kernel test cases
=================

This list comprises some test cases that are useful to identify missing
kernel configuration.

Docker
------

Launch a container

.. code-block:: text

    sudo apt-get install docker.io
    sudo docker pull ubuntu:noble
    sudo docker run -ti ubuntu:noble
    exit
    sudo container prune -f
    sudo image rm ubuntu:noble

Firewall
--------

Add Firewall rules

.. code-block:: text

    sudo ufw enable
    sudo ufw status
    sudo ufw allow from 192.168.0.0/16 proto udp port 1714:1764 comment 'KDE Connect'
    sudo ufw allow from 192.168.0.0/16 proto tcp port 1714:1764 comment 'KDE Connect'
    sudo ufw status numeric
    sudo ufw disable
    sudo ufw status

LXC, LXD
--------

Launch a container

.. code-block:: text

    sudo snap install --channel latest/stable lxd
    # If the snap is already installed
    # sudo snap refresh --channel latest/stable lxd
    lxc init --auto
    lxc launch ubuntu:24.04 noble1
    lxc shell noble1
    sudo poweroff
    lxc delete noble1

Snapcraft
---------

Build a snap

.. code-block:: text

    sudo snap install --classic snapcraft
    sudo ufw disable
    lxd init
    git clone https://git.code.sf.net/p/sispmctl/git sispmctl
    cd sispmctl/
    snapcraft --use-lxd --verbose
    find . -name '*.snap'
