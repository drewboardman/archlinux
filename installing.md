Installing ArchLinux on VB
==========================
  - Download the `archlinux-2017.03.01-dual.iso` (obviously most recent) from
    some [mirror](https://www.archlinux.org/download/).

VB specific config
------------------
  1. choose *Arch Linux 64 bit*
  2. choose whatever for memory (I originally used 8G)
  3. select *virtual hard disk* and *VDI*
    * again use whatever capacity you think you'll need
  4. Start the VB with the `.iso` you downloaded

Initial Setup of System
-----------------------
The first step is to partition the disks. I used
[*parted*](https://wiki.archlinux.org/index.php/GNU_Parted) for this.
### Partitioning the disk (`parted`)
  1. `parted` should already be a binary available when you use the arch iso,
     but just check `which parted` to be sure
  2. You need to enter the `parted` CLI
    * you can check the disk name with `fdisk -l` or `lsblk`
```bash
parted /dev/sda
```

    - Now you should be in the CLI. When you partition the disk, you're going to see
    that you need to assign it a "label type". The labels that you need to be concerned
    with are *gpt* and *msdos*. I'm not entirely sure the nuanced difference between
    the two label types, but the newest type is GPT (so YOLO just pick GPT).

    - Also, don't forget that you can't skip to partitioning until you run the `mklabel` command

  3. Use the `mklabel` command to set the *label type* for the disk partitions
```bash
(parted) mklabel
New disk label type? gpt
```

  4. Now just follow the instructions to make two partitions to later be used
     for `/` and `/home`. The general form of the command is `(parted) mkpart part-type fs-type
start end`. The first command below starts at 513M so that there is 513M room
for the bootloader you are going to configure later.
```bash
(parted) mkpart primary ext4 513M 50%
(parted) mkpart primary ext4 50% 100%
```
