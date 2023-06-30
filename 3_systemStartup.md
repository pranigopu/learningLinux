# System startup in Linux

## Step 1

- Upon power on, the BIOS (basic input output system)
	- Initialises the screen and keyboard
	- Tests the main memory
- This process is also called POST (power on self-test). Note that the BIOS software is stored on a ROM chip in the motherboard.

## Step 2

- After POST, the system control passes onto the bootloader (which is usually stored on one of the system's hard disks).
- There are multiple bootloaders available for Linux:
	- GRUB (grand unified bootloader)
	- Das-U Boot (for booting on embedded devices)
	- LILO (Linux loader)
	- LOADLIN (load Linux)

### About the bootloader
#### 1st stage bootloader
A bootloader, also called a boot manager, is a small program that places the OS into the main memory. The bootloader resides in a record on the 1st sector of the hard disk, called the master boot record (MBR, size ~ 512 B).

Using the partition table (i.e. the table listing the partitions available in the main memory), the bootloader finds a bootable partition (i.e. a partition wherein the OS can be loaded).

### 2nd stage bootloader
Upon finding a bootable partition, the 1st stage bootloader searches and loads the 2nd stage bootloader (ex. GRUB) into the RAM. The 2nd stage bootloader lies in the directory `/boot`.

## Step 3
The 2nd stage bootloader presents a display to choose the OS to be loaded if there are multiple OS's available in the system. When the OS is chosen, the bootloader loads the kernel of that OS into RAM and passes control to it.

## Step 4
Kernels are almost always compressed, so it first decompresses itself when loaded into RAM. After this, the kernel

- Analyses the system hardware and initialises and configures the hardware attached to the system, including:
	- Processors
	- I/O subsystems
	- Storage devices
- Initialises and configures the main memory
- Mounts the root filesystem
- Loads some necessary user applications

### Intialising the filesystem
To enable the mounting of the root filesystem, the kernel loads an initial filesystem ("initramfs") into RAM. Mounting a filesystem attaches that filesystem to a directory (called a mount point, ultimately connected to the root directory) and thus makes it available to the system. The root directory is always mounted.

The initramfs is used as the 1st root filesystem that your machine has access to, and is used for mounting the real root filesystem ("rootfs") which has all your data. *The initramfs carries the modules needed for mounting your rootfs*.

The initramfs is a root filesystem that is embedded into the kernel and loaded at an early stage of the boot process. "initramfs" stands for "initial RAM File System". On modern Linux systems, it is generally stored in a file under the `/boot` directory. The kernel version for which the initramfs is built will be included in the file name, and a new initramfs is generated every time a new kernel is installed.

## Step 5
After the initialisation and configuration stage, the kernel begins the `sbin/init` process, which becomes parent process to every subsequent process in the system. Except for kernel processes, every process in the system traces its origin to `sbin/init`. Kernel processes are managed by the kernel directly. They manage internal OS details.

`sbin/init` is also responsible for keeping the system running and, when necessary, shutting it down. `sbin/init` is also responsible for managing other non-kernel processes (foreground and background) as well as user-logins for users.

# Further reading
- EFI and UEFI systems

> Written with [StackEdit](https://stackedit.io/).
