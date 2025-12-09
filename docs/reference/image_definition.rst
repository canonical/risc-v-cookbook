.. SPDX-License-Identifier: CC-BY-SA-4.0

Image-definition.yaml fields
============================

The available fields of the image-definition.yaml file are defined in
https://github.com/canonical/ubuntu-image/blob/main/internal/imagedefinition/image_definition.go.
A description is provided in
https://github.com/canonical/ubuntu-image/blob/main/internal/imagedefinition/README.rst.

Here is a list of the field hierarchy as of ubuntu-image 3.6.0:

.. code-block:: text

    (imagedefinition.ImageDefinition)
    |-name (string)
    |-display-name (string)
    |-revision (int)
    |-architecture (string)
    |-series (string)
    |-kernel (string)
    |-gadget (*imagedefinition.Gadget)
    | |-ref (string)
    | |-target (string)
    | |-branch (string)
    | |-type (string) enum:git,directory,prebuilt
    | |-url (string) type=string,format=uri
    |-model-assertion (string) type=string,format=uri
    |-rootfs (*imagedefinition.Rootfs)
    | |-components ([]string)
    | |-archive (string)
    | |-flavor (string)
    | |-mirror (string)
    | |-pocket (string) enum:release,Release,updates,Updates,security,Security,proposed,Proposed
    | |-seed (*imagedefinition.Seed) oneof_required=Seed
    | | |-branch (string)
    | | |-urls ([]string) type=array,format=uri
    | | |-names ([]string)
    | | |-vcs (*bool)
    | |-tarball (*imagedefinition.Tarball) oneof_required=Tarball
    | | |-url (string) type=string,format=uri
    | | |-gpg (string) type=string,format=uri
    | | |-sha256sum (string) minLength=64,maxLength=64
    | |-archive-tasks ([]string) oneof_required=ArchiveTasks
    | |-sources-list-deb822 (*bool)
    |-customization (*imagedefinition.Customization)
    | |-components ([]string)
    | |-pocket (string) enum:release,Release,updates,Updates,security,Security,proposed,Proposed
    | |-installer (*imagedefinition.Installer)
    | | |-preseeds ([]string)
    | | |-layers ([]string)
    | |-cloud-init (*imagedefinition.CloudInit)
    | | |-meta-data (string)
    | | |-user-data (string)
    | | |-network-config (string)
    | |-extra-ppas ([]*imagedefinition.PPA)
    | | |-name (string) pattern=^[a-zA-Z0-9_.+-]+/[a-zA-Z0-9_.+-]+$
    | | |-auth (string) pattern=^[a-zA-Z0-9_.+-]+:[a-zA-Z0-9]+$
    | | |-fingerprint (string)
    | | |-keep-enabled (*bool)
    | |-extra-packages ([]*imagedefinition.Package)
    | | |-name (string)
    | |-extra-snaps ([]*imagedefinition.Snap)
    | | |-name (string)
    | | |-revision (int) type=integer
    | | |-store (string)
    | | |-channel (string)
    | |-fstab ([]*imagedefinition.Fstab)
    | | |-label (string)
    | | |-mountpoint (string)
    | | |-filesystem-type (string)
    | | |-mount-options (string)
    | | |-dump (bool)
    | | |-fsck-order (int)
    | |-manual (*imagedefinition.Manual)
    | | |-make-dirs ([]*imagedefinition.MakeDirs)
    | | | |-path (string)
    | | | |-permissions (uint32)
    | | |-copy-file ([]*imagedefinition.CopyFile)
    | | | |-destination (string)
    | | | |-source (string)
    | | |-execute ([]*imagedefinition.Execute)
    | | | |-path (string)
    | | |-touch-file ([]*imagedefinition.TouchFile)
    | | | |-path (string)
    | | |-add-group ([]*imagedefinition.AddGroup)
    | | | |-name (string)
    | | | |-id (string)
    | | |-add-user ([]*imagedefinition.AddUser)
    | | | |-name (string)
    | | | |-id (string)
    | | | |-password (string)
    | | | |-password-type (string) enum:text,hash
    |-artifacts (*imagedefinition.Artifact)
    | |-img (*[]imagedefinition.Img)
    | | |-name (string)
    | | |-volume (string)
    | |-iso (*[]imagedefinition.Iso)
    | | |-name (string)
    | | |-volume (string)
    | | |-xorriso-command (string)
    | |-qcow2 (*[]imagedefinition.Qcow2)
    | | |-name (string)
    | | |-volume (string)
    | |-manifest (*imagedefinition.Manifest)
    | | |-name (string)
    | |-filelist (*imagedefinition.Filelist)
    | | |-name (string)
    | |-changelog (*imagedefinition.Changelog)
    | | |-name (string)
    | |-rootfs-tarball (*imagedefinition.RootfsTar)
    | | |-name (string)
    | | |-compression (string) enum:uncompressed,bzip2,gzip,xz,zstd
    |-class (string) enum:preinstalled,cloud,installer

Execution sequence
------------------

The manual steps are executed in this sequence:

* make-dirs
* copy-file
* execute
* touch-file
* add-group
* add-user

Example
-------

Here is an example:

.. code-block:: yaml

   ---
   name: ubuntu-server-riscv64-preinstalled
   display-name: Ubuntu Server RISC-V preinstalled
   revision: 1
   architecture: riscv64
   series: noble
   class: preinstalled
   kernel: linux-image-generic
   gadget:
     url: "https://github.com/canonical/risc-v-gadget.git"
     type: "git"
     branch: "main"
   rootfs:
     archive: ubuntu
     sources-list-deb822: true
     components:
       - main
       - restricted
       - universe
       - multiverse
     mirror: "http://ports.ubuntu.com/ubuntu-ports/"
     pocket: updates
     seed:
       urls:
         - "https://git.launchpad.net/~ubuntu-core-dev/ubuntu-seeds/+git/"
       branch: noble
       names:
         - cloud-image
         - server
         - minimal
         - standard
   customization:
     extra-packages:
       - name: "flash-kernel"
       - name: "grub-efi-riscv64"
     extra-snaps:
       - name: snapd
     fstab:
       - label: "writable"
         mountpoint: "/"
         filesystem-type: "ext4"
         dump: false
         fsck-order: 1
       - label: "esp"
         mountpoint: "/boot/efi"
         filesystem-type: "vfat"
         mount-options: "defaults"
         dump: false
         fsck-order: 1
   artifacts:
     img:
       - name: ubuntu-24.04-preinstalled-server-riscv64.img
     manifest:
       name: ubuntu-24.04-preinstalled-server-riscv64.manifest
