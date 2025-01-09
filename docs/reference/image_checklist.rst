.. SPDX-License-Identifier: CC-BY-SA-4.0

Checklist for Ubuntu images
===========================

.. list-table::
   :header-rows: 1

   * - Requirement
     - Test commands
     - Checks
   * - Login via SSH
     - .. prompt:: bash $ auto

          $ systemctl enable ssh.service
          $ systemctl status ssh.service
     - * The SSH service can be started.
       * Login via SSH is possible.
   * - The Ubuntu firewall is working.
     - .. prompt:: bash $ auto

          $ sudo ufw enable
	  $ sudo ufw allow ssh
	  $ sudo ufw status numbered
     - * The firewall can be enabled.
       * SSH rules are added.
       * SSH login is still possible.
   * - Packages can be installed.
     - .. prompt:: bash $ auto

          $ sudo apt-get update
	  $ sudo sudo apt-get install hello
	  $ /usr/bin/hello
     - * The repository information could be downloaded.
       * The installation of the package succeeds.
       * The hello command is functional.
   * - The image can be upgraded.
     - .. prompt:: bash $ auto

          $ sudo apt-get update
	  $ sudo apt-get dist-upgrade
     - * The repository information could be downloaded.
       * The installation of updates succeeds.
       * After reboot the image still has full functionality,
         e.g. the desktop is still available.
   * - Snaps can be installed.
     - .. prompt:: bash $ auto

          $ sudo snap install hello
	  $ /snap/bin/hello
     - * The snap is installed successfully.
       * The hello command is functional.
   * - LXD can be used.
     - .. prompt:: bash $ auto

          $ sudo snap install lxd
	  $ sudo adduser $USER lxd
	  $ logout
	  # login
	  $ lxd init --minimal
	  $ lxc image list ubuntu:
	  $ lxc launch ubuntu:24.04 mycontainer
	  $ lxc shell mycontainer
	  $ logout # (in container)
	  $ lxc list
	  $ lxc stop mycontainer
	  $ lxc delete mycontainer
	  $ lxc list mycontainer
     - * The snap is installed successfully.
       * A container can be created.
       * The container can be started.
       * The shell can be reached.
       * The container can be deleted.
