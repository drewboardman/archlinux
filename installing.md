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
  1. 
