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
### Partitioning the disk (`cfdisk`)
  1. In my opinion, `cfdisk` is way easier than `parted`. The reason is that
     doing *start/end* with percentages gets weird when you add a swap into the
mix
  2. Run `cfdisk /dev/sda`. Pick `dos`
  3. There is a decent tutorial
     [here](https://www.ostechnix.com/install-arch-linux-latest-version/).
Basically just follow the steps in the gui. You're going to end up with 3
partitions - that look like there are really 4. Here is a brief overview
    - Boot: `New -> select size -> select primary -> make bootable -> write ->
      'yes'`
    - Swap: `New -> select size -> select primary -> write -> 'yes'
    - Home: This one is a bit trickier. It's a partion *inside* an extended
      partition. So it has 2 parts.
      - `New -> select size -> select extended.` Now select the `Free Space` that should appear under this partition. `New -> select size -> write -> 'yes'`

### Formatting the file system
  - You need to format the `/` and `/home` partitions. The `/` is going to be
    the bootable & primary. The `/home` is going to be the primary partition
that's *under* an extended. In my case
these were `/dev/sda1` and `/dev/sda5`.
```bash
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda5
```

### Mounting and Swap
  - Set the swap directory
```bash
mkswap /dev/sda2
swapon /dev/sda2
```
  - You need to mount the home and root directories
```bash
mount /dev/sda1 /mnt
mkdir /mnt/home
mount /dev/sda5 /mnt/home
```

Installing Arch on your filesystems that you created
---------------
```bash
pacstrap /mnt base base-devel
```
  - You also need to create a *File System Table* or `fstab`. There is a pretty
    good quote about what this thing is:

>Fstab is configured to look for specific file systems and mount them
>automatically in a desired way each and every time, preventing a myriad of
>disasters from occurring.

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Additional Steps
----------------
  1. Install vim `pacman -S vim`
  2. Change root into the `/mnt` file system

```bash
arch-chroot /mnt
```

  3. Set hostname with
```bash
echo <hostnameYouChoose> > /etc/hostname`
hostname -F /etc/hostname
```
    - also you want to change the /etc/hosts to have
```
127.0.0.1  localhost.localdomain   localhost
::1        localhost.localdomain   localhost
127.0.1.1  myhostname.localdomain  myhostname
```

BootLoader
----------
  1. `pacman -S grub os-prober`
  2. `grub-install /dev/sda`
  3. `grub-mkconfig -o /boot/grub/grub.cfg`
    - This might throw up some nonsense about LVM. Ignore that.

Final
-----
Finally, exit from the chrrot, unmount the partitions and reboot your Arch
Linux. Make sure you have removed the installation media too.

```bash
exit
umount -R /mnt
reboot
```
  - Select Existing OS

Part 2
======
  - Your networking might not be working at this point. If you're running Arch
    inside VirtualBox, run the command `systemctl enable dhcpcd@enp0s3.service` 

  1. Once you get your networking shit together, update packages `pacman -Syyu`
  2. Enable multilib in `/etc/pacman.conf`
  3. Go into `/etc/pacman.d/mirrorlist` and move some of the United States
     mirrors to the top (Arch prioritizes mirrors from top>bottom)
  4. Install `zsh` (you're going to set it as the default shell for your user)
    - `pacman -S zsh`

### Creating a user
  - According to the wiki

>Do not worry about adding the user to other groups now. If later on you will
>install a particular application, or perform a specific configuration, that
>requires explicitly adding the user to a group, the wiki will remind you to do
>it in the specific article that you will be following.

So we're just going to add a user to the `wheel` group right now

```bash
useradd -m -G wheel -s /bin/zsh drew
```

#### Adding new user to `sudo`ers
  1. Install `sudo` with `pacman -S sudo`
  2. Add your user as to `sudo`ers
```bash
visudo
```
Find and uncomment the following line:

```
 %wheel ALL=(ALL) ALL
```
