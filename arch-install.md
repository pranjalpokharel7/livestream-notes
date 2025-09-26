# day 1

**note:** learn more about KVM, maybe go through it on a architecture level in the next stream

**note:** remember to verify PGP signature for ISO installation (also for other software that you install anytime - your package manager does this automatically!)

- create a QEMU image (UEFI setting, 4GB memory - adjustable)

```bash
qemu-img create -f qcow2 arch.qcow2 30G
```

- boot iso into image
```bash
qemu-system-x86_64 -m 4G -cdrom archlinux-2025.06.01-x86_64.iso -boot d -drive file=arch.qcow2,format=qcow2 -enable-kvm -bios /usr/share/OVMF/OVMF_CODE.fd
```

**note:** the packages you have available during installation aren't necessarily available once you boot into the installed system (live environment vs installed environment)

next steps are to follow the archwiki

24:00 - start arch installation in qemu
34:00 - restart in UEFI mode 

-- skipped network configuration because our host provides access to network by default in QEMU virtualization -> this only applies to "live environment" -- arch is so much barebones that it doesn't even come with the NetworkManager!

deadlock: i don't have package to manage network -> to install said package I need to connect to a network (which is not wifi)

possible way out? -> to connect an ethernet cable (but I don't have an ethernet cable at home)

41:00 - disk partitioning
54:00 - format the partitions
59:00 - mounting the partitions
1:03:00 - installing packages (skipped really)
1:09:00 - configuring system
1:27:00 - installing bootloader (GRUB)
1:33:00 - **received my first donation!**
1:42:00 - reboot into new system

**today's mistake:** forgot to setup networkmanager daemon

> The [fstab(5)](https://man.archlinux.org/man/fstab.5) file can be used to define how disk partitions, various other block devices, or remote file systems should be mounted into the file system.

creating the partitions -> is like dividing a house into rooms -> but now you need to fill up those rooms (eg, what good is a toilet without taps and commode?) -> to populate the partitions, we format the disk

formatting your USB drive -> format it based on a certain filesystem

"The ISO kernel is just for the installation environment, it's separate from the kernel you'll install on your actual system." - that is why the arch ISO is so small
```bash
pacstrap -K /mnt base linux linux-firmware
```


# day 2

speed-running arch installation (mostly because we want to get to a state where the installation is usable for daily basis)

**target:** browse my personal website from firefox

note: 
- talk about zram at the end???
- try different partition types in the next stream (LVM, encrypted, etc.)

11:00 - start following instructions from archwiki
16:00 - start disk partition

```
efi partition - 1GB
linux partition - rest
(no swap - we'll use zram)
```

-- new stream
06:00 - restart at partitioning
10:30 - formatting the partition
13:00 - mounting the partitions
15:42 - installing packages

note: make a youtube video instead because livestream is getting frustrating

**mr. claude -**

If you're running a VM right now and want to suspend it:

1. Access QEMU monitor (Ctrl+Alt+2 in SDL/GTK interface)
2. Type: `migrate "exec:cat > /path/to/save/state.img"`
3. Wait for completion, then quit QEMU
4. Later, restart with: `qemu-system-x86_64 [original-args] -incoming "exec:cat /path/to/save/state.img"`


# day 3

- restoring from snapshot
```bash
qemu-system-x86_64 -m 4G -cdrom archlinux-2025.06.01-x86_64.iso -boot d -drive file=arch.qcow2,format=qcow2 -enable-kvm -bios /usr/share/OVMF/OVMF_CODE.fd -incoming "exec:cat arch_state_package_install.img"
```

configuring grub
**note:** *you need to be inside the chroot when running `grub-install`*

```
$ pacman -Syu grub efibootmgr
$ grub-install --target=x86_64-efi --efi-directory=boot --bootloader-id=GRUB
$ grub-mkconfig -o /boot/grub/grub.cfg
```