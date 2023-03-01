# opal20_ulocker
SSD Opal 2.0 Encryption Live Image Unlocker

Create image with `livecd-creator fedora-opal.ks` in /usr/share/spin-kickstarts folder. The resulting image will be placed there. You can inspect this image with losetup -fP <image> (this will discover the partition).

This image doesn't allow to logon from getty.

It has /dev/nvme0 and /dev/sda hardcoded.

Extract squashfs filesystem from resulting image and write it directly to the USB flash drive partition like /dev/sdX3 (cat <squashimage> >/dev/sdX3).
Create EFI partition (/dev/sdX1) and /boot (/dev/sdX2) partition and use opal.conf for grub in `/boot/loaded/` below.
EFI you can copy from your current fedora just like  /boot, just rename latest kernel and initrd to `vmlinuz` and `initrd0.img`.
When booting from UEFI, use grubx64.efi to boot and then you can hold shift to see grub menu.
You can find PARTUUID of /dev/sdX3 in folder `/dev/disk/by-partuuid/`.
Format USB flash drive with GPT partition scheme.

opal.loader:
  title Opal
  version 5.18
  linux /vmlinuz0
  initrd /initrd0.img
  options root=live:PARTUUID=a0933336-1a0d-4a3f-82b2-83401b6bd243 ro libata.allow_tpm=1 selinux=1 rd.live.image quiet rhgb noefi
  grub_users $grub_users
  grub_arg --unrestricted
  grub_class fedora
