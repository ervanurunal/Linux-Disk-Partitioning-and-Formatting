## Disk Partitioning & Formatting in Linux

---

### Overview

In this lab, I practiced **partitioning and formatting a disk drive in Linux**.

I learned how to:

* Identify block devices and disks
* Determine whether a disk is mounted or unmounted
* View disk partitions
* Create and delete partitions using `fdisk`
* Create a swap partition
* Change a partition type
* Format a partition using `mkfs`
* Create a mount point
* Mount a partition to the Linux filesystem
* Verify disks, partitions, and filesystems

These are important skills for an **IT Support Specialist** because disk and filesystem management are common Linux administration tasks.

---

### Tools & Resources

* Linux Virtual Machine (Qwiklabs)
* Linux Terminal / Shell
* `lsblk`
* `df`
* `fdisk`
* `mkfs`
* `mount`
* Linux `ext4` filesystem
* Linux Swap partition

### Lab Type 
Linux / IT Support Hands-On Lab

---
---

### 1. Examine Block Devices

#### Command

```bash
lsblk
```

![1](https://i.imgur.com/vTieY1L.png)

#### *Purpose*

`lsblk` displays information about block devices attached to the Linux system.

It shows:

* Device names
* Disk sizes
* Partitions
* Mount points
* Device hierarchy

---

### 2. View Mounted Disks

#### Command

```bash
df -h
```

![2](https://i.imgur.com/HH4BlFN.png)

#### *Purpose*

The `df` command displays available and used disk space.

The `-h` option means **human-readable**, making sizes easier to understand.

---

### 3. Examine Disk Partitions

#### List all partitions

```bash
sudo fdisk -l
```

![3](https://i.imgur.com/R8TeL8x.png)

#### *Purpose*

`fdisk -l` displays partition information for the disks connected to the system.

It can show:

* Disk size
* Partition numbers
* Start and end sectors
* Partition sizes
* Partition types

---

#### List Partitions on a Specific Disk

Replace `[MOUNTED DRIVE]` with the appropriate device.

```bash
sudo fdisk -l /dev/[MOUNTED DRIVE]
```

![4](https://i.imgur.com/xgvq3N2.png)

---

### 4. Open fdisk in Interactive Mode

Before making changes, identify the **unmounted drive**.

```bash
sudo fdisk /dev/[UNMOUNTED DRIVE]
```

![5](https://i.imgur.com/QcA4w32.png)

---

### 5. Explore fdisk

Inside `fdisk`, commands are entered using individual letters.

#### Display Help

```text
m
```

The `m` command displays the available `fdisk` commands.

![6](https://i.imgur.com/WjpljcQ.png)

---

#### Display Partition Information

```text
p
```

The `p` command prints the current partition table.

![7](https://i.imgur.com/8pxRKEf.png)

---

#### Exit Without Saving

```text
q
```

The `q` command quits `fdisk` without saving changes.

This is useful if I want to cancel my changes.

---

### 6. Delete Existing Partitions

The lab disk initially contains multiple partitions.

To delete a partition:

```text
d
```

`fdisk` asks for the partition number.

The lab requires deleting all existing partitions, starting with the last partition.

For example:

```text
d
12
```

Then continue:

```text
d
11
```

```text
d
10
```

Continue until partitions `1–12` have been deleted.

![8](https://i.imgur.com/h83Tygu.png)


#### Important

Partition deletion is **destructive**.

Deleting partitions can cause data loss, so disk information should always be backed up before performing these operations on a real system.

---

### 7. Create the First Partition

After deleting the existing partitions, create a new partition:

```text
n
```

Select the default partition number by pressing:

```text
Enter
```

Select the default first sector:

```text
Enter
```

For the first partition, enter the last sector:

```text
2097200
```

This creates approximately a **1 GB partition**.

The partition is initially created as a Linux filesystem partition.

![9](https://i.imgur.com/FOsEqcu.png)

---

### 8. Create the Second Partition

Create another partition:

```text
n
```

Select the next partition number:

```text
Enter
```

Select the default first sector:

```text
Enter
```

Select the default last sector:

```text
Enter
```

The remaining disk space becomes the second partition.

![11](https://i.imgur.com/HqfS2cI.png)

---

### 9. Change Partition Type

The first partition needs to be changed to a **Linux swap** partition.

Inside `fdisk`, use:

```text
t
```

Select partition:

```text
1
```

To view available partition types:

```text
L
```

In this lab, select:

```text
19
```

This changes partition 1 to:

```text
Linux swap
```

![12](https://i.imgur.com/6GUQoP0.png)
![13](https://i.imgur.com/rwa0JZv.png)


---

### 10. Verify the Partition Table

Before saving the changes, verify the configuration:

```text
v
```

This checks the partition table for errors.

![14](https://i.imgur.com/JQiz4Rh.png)

---

### 11. Save the Changes

To write the new partition table to the disk:

```text
w
```

The changes are now committed to the disk.

![15](https://i.imgur.com/VgGqEPL.png)

---

### 12. Verify the New Partitions

Exit `fdisk` and run:

```bash
lsblk
```

This should show the newly created partitions.

![16](https://i.imgur.com/jwlVtds.png)

---

### 13. Format the Second Partition

The second partition will use the **ext4** filesystem.

First, identify the correct partition with:

```bash
lsblk
```

Then format partition 2:

```bash
sudo mkfs -t ext4 /dev/sdb2
```

![17](https://i.imgur.com/9zHxgPJ.png)

---

### 14. Create a Mount Point

Create the directory where the partition will be mounted:

```bash
sudo mkdir /home/my_drive
```

The directory acts as the access point for the new filesystem.

---

### 15. Mount the Partition

Mount the second partition:

```bash
sudo mount /dev/[UNMOUNTED DRIVE]2 /home/my_drive
```

![18](https://i.imgur.com/d3wIEpu.png)

---

### 16. Verify the Filesystem

Run:

```bash
lsblk
```

![19](https://i.imgur.com/d3wIEpu.png)

---
---

### Commands Learned

| Command          | Purpose                                                |
| ---------------- | ------------------------------------------------------ |
| `lsblk`          | Display block devices, partitions, and mount points    |
| `df -h`          | Display filesystem disk usage in human-readable format |
| `fdisk -l`       | List disk partition information                        |
| `fdisk /dev/sdX` | Open a disk in fdisk interactive mode                  |
| `m`              | Display fdisk help                                     |
| `p`              | Display the partition table                            |
| `d`              | Delete a partition                                     |
| `n`              | Create a new partition                                 |
| `t`              | Change partition type                                  |
| `L`              | List partition types                                   |
| `v`              | Verify partition table                                 |
| `w`              | Write/save partition changes                           |
| `q`              | Quit fdisk without saving                              |
| `mkfs -t ext4`   | Create an ext4 filesystem                              |
| `mkdir`          | Create a directory                                     |
| `mount`          | Mount a filesystem                                     |

---
---

### Cybersecurity Relevance

Although this was an IT Support lab, disk management is also relevant to cybersecurity.

Understanding filesystems and storage helps with:

* System administration
* Log storage
* Security monitoring
* Incident response
* Disk investigation
* Linux permissions
* Evidence preservation
* Storage troubleshooting

For a **SOC Analyst**, understanding Linux storage can be useful when investigating logs, suspicious files, system activity, and compromised systems.

---
---

### Troubleshooting Notes

#### Disk does not appear

Check connected block devices:

```bash
lsblk
```


#### Need more partition information

Run:

```bash
sudo fdisk -l
```


#### Need to check mounted filesystems

Run:

```bash
df -h
```


#### Need to check whether a partition has a filesystem

Run:

```bash
lsblk -f
```


#### Mount command fails

Check:

1. The partition name is correct.
2. The filesystem was created.
3. The mount directory exists.
4. The partition is not already mounted.

---
---

### Key Concepts

#### Block Device

A block device is a device that stores and accesses data in fixed-size blocks.

Examples include:

```text
/dev/sda
/dev/sdb
```

#### Partition

A partition is a logical section of a physical disk.

Example:

```text
/dev/sda1
/dev/sda2
```

#### Filesystem

A filesystem determines how data is organized and stored on a partition.

Example:

```text
ext4
```

#### Mount Point

A mount point is a directory through which a filesystem becomes accessible.

Example:

```text
/home/my_drive
```

#### Swap

Swap is disk space that Linux can use as additional virtual memory.

---
---

### Skills Demonstrated

**Technical Skills**

* Linux command line
* Disk management
* Partition management
* Filesystem management
* `fdisk`
* `mkfs`
* `lsblk`
* `df`
* Mounting filesystems
* Linux swap configuration

**IT Support Skills**

* Troubleshooting storage
* System administration
* Command-line troubleshooting
* Filesystem identification
* Hardware/storage management

**Cybersecurity Foundation**

* Linux system administration
* Filesystem awareness
* Storage and log management
* Incident-response fundamentals
* Security investigation fundamentals

---
---

### Final Takeaway

This lab gave me hands-on practice managing Linux storage from the command line. I learned how to identify disks, create and modify partitions, format filesystems, and mount storage so that Linux can access it.

These skills provide a foundation for both **IT Support** and **Cybersecurity/SOC Analyst** roles.
