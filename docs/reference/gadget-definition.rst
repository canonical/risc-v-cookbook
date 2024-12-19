.. SPDX-License-Identifier: CC-BY-SA-4.0

Gadget.yaml fields
==================

The available fields of the gadget.yaml file are defined in
https://github.com/canonical/snapd/blob/master/gadget/gadget.go

A description is provided in section Gadget.yaml - Specification of
https://snapcraft.io/docs/the-gadget-snap.

Here is a list the relevant part of the field hierarchy as of Snapd 2.67:

.. code-block:: text

    -volumes,omitempty (map[string]*Volume)
     |-<Key> (string)
     | <Value> (*Volume)
     | |-schema (string) enum:emmc,gpt,mbr
     | |-bootloader (string) enum:android-boot,grub,lk,piboot
     | |-id (string)
     | |-structure ([]VolumeStructure)
     | | |-name (string)
     | | |-type (string)
     | | |-filesystem (string) enum:ext4,vfat,vfat-16,vat-32
     | | |-filesystem-label (string)
     | | |-min-size (Size)
     | | |-size (Size)
     | | |-offset (*Offset)
     | | |-offset-write (*RelativeOffset)
     | | | |-relative-to (string)
     | | | |-offset (Offset)
     | | |-role (string) enum:system-boot,system-data,system-seed,system-seed-null,system-save
     | | |-id (string)
     | | |-content ([]VolumeContent)
     | | | |-source (string)
     | | | |-target (string)
     | | | |-image (string)
     | | | |-offset (*Offset)
     | | | |-size (Size)
     | | | |-unpack (bool)


The type field can have as value:

* The string :samp:`bare`.
  This indicates an unformatted partition, e.g. for placing firmware.
* A two digit partition type for MBR partition tables, e.g. :samp:`EF`
* A partition type GUID for GPT partition tables,
  e.g. :samp:`C12A7328-F81F-11D2-BA4B-00A0C93EC93B`
* a combination of MBR and GPT partition type
  e.g. :samp:`EF,C12A7328-F81F-11D2-BA4B-00A0C93EC93B`

Size, min.size, and offset fields can use an 'M' or 'G' postfix to indicate
mebibytes or gibibytes. The following would be valid values for size:

* 1048576
* 4M
* 16G

Here is an example:

.. code-block:: yaml

    ---
    volumes:
      riscv:
        schema: gpt
        bootloader: grub
        structure:
          - name: starfive_visionfive_2_u-boot-spl
            type: 2E54B353-1271-4842-806F-E436D6AF6985
            size: 1M
            content:
              - image: u-boot-starfive/u-boot-spl.bin.normal.out
          - name: starfive_visionfive_2_u-boot
            # random value for type
            type: 816E2DB1-6FE4-4797-B3FF-2DE37CB85318
            size: 16M
            content:
              - image: u-boot-starfive/u-boot.itb
          - name: EFI System
            type: C12A7328-F81F-11D2-BA4B-00A0C93EC93B
            filesystem: vfat
            filesystem-label: esp
            size: 100M
            role: system-boot
            content:
              - source: grub/grubriscv64.efi
                target: EFI/boot/bootriscv64.efi
              - source: grub/grubriscv64.efi
                target: EFI/ubuntu/grubriscv64.efi
              - source: grub/grub.cfg
                target: EFI/debian/grub.cfg
              - source: grub/grub.cfg
                target: EFI/ubuntu/grub.cfg
              - source: dtb/
                target: dtb/
          - name: CIDATA
            type: 0FC63DAF-8483-4772-8E79-3D69D8477DE4
            filesystem: vfat-16
            filesystem-label: CIDATA
            size: 4M
            content:
              - source: cidata/meta-data
                target: meta-data
              - source: cidata/user-data
                target: user-data
          - name: Root Partition
            type: 0FC63DAF-8483-4772-8E79-3D69D8477DE4
            filesystem: ext4
            filesystem-label: writable
            size: 3072M
            role: system-data
