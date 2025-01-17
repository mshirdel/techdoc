LVM is a Logical Volume Manager for the Linux operating system. There are now two version of LVM for Linux:
- LVM 2 - The latest and greatest version of LVM for Linux.
    
    LVM 2 is almost completely backward compatible with volumes created with LVM 1. The exception to this is snapshots (You must remove snapshot volumes before upgrading to LVM 2)
    
    LVM 2 uses the device mapper kernel driver. Device mapper support is in the 2.6 kernel tree and there are patches available for current 2.4 kernels.
    
- LVM 1 - The version that is in the 2.4 series kernel

LVM 1 is a mature product that has been considered stable for a couple of years. The kernel driver for LVM 1 is included in the 2.4 series kernels, but this does not mean that your 2.4.x kernel is up to date with the latest version of LVM.

### What is Logical Volume Management?
Logical volume management provides a higher-level view of the disk storage on a computer system than the traditional view of disks and partitions. This gives the system administrator much more flexibility in allocating storage to applications and users.

Storage volumes created under the control of the logical volume manager can be resized and moved around almost at will, although this may need some upgrading of file system tools.

The logical volume manager also allows management of storage volumes in user-defined groups, allowing the system administrator to deal with sensibly named volume groups such as "development" and "sales" rather than physical disk names such as "sda" and "sdb".