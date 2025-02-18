# Chapter 3: Navigating the Linux Filesystem

Before going to learn what linux filesystem is? what hierarchy it follows? What are the types of files? How exactly file permissions exactly works? How to mount or unmount a filesystem? we need to understand the basic terminologies used in this chapter

## Terminologies
- **Filesystem:** In the context of Linux, a `filesystem` refers to a specific method of organizing and storing files on a storage device, like a hard drive, where data is structured in a hierarchical directory system, with the most commonly used filesystem on Linux being "ext4" (fourth extended filesystem) which offers a balance between performance and reliability; essentially, it's how the operating system manages and accesses files on the disk. 
- **Hierarchy:** Refers to a tree-like structure where all files and directories are organized starting from a single root directory ("/") and branching out into subdirectories
- **Mounting:** Attaching a separate file system to a specific directory within the existing file hierarchy, making its contents accessible as if they were part of the main file system at that designated point (called a mount point); essentially, it allows you to access files from a different storage device or partition as if they were located directly on your current system. 
- **Unmounting:** Refers to detach a previously mounted file system from its designated mount point, effectively making it inaccessible within the current file system hierarchy, essentially "disconnecting" the external storage device from the system until it's mounted again; this is done using the "umount" command to ensure data integrity by flushing any pending write operations before removing the file system access. 

## 3.1 Introduction to the Linux Filesystem
Linux follows the FHS (Filesystem Hierarchy Standard), defining a structured directory systems. Basically it is devided into the three layers:

1.  **Logical Filesystem:** The Logical File System act as a the interface between the user and the applications making sure that application can interact with the file system in a way that aligns with the user expectations. This layer deals with opening, reading, and closing files.
2.  **Virtual Filesystem:** The Virtual Filesystem (VFS) is a crucial layer that enables various operations on multiple instances of physical file systems. It provides a standard interface, abstracting the underlying complexities and ensuring compatibility and cohesion between different file system implementations.
3.  **Physical Filesystem:** This layer actually directly interacts with the hardware components. It is responsible for tangebile management and storage of physical memory blocks on the disk. This layer ensures the efficient allocation and utilization of physical storage resources, contributing to the overall performance and reliability of the file system.

![The Architecture of a File System](https://media.geeksforgeeks.org/wp-content/uploads/20231128122313/Linux.png)

### Charactericstics of File System

Following characteristics collectively define how a file system operates and interacts with the underlying storage hardware and the user.

#### 1. Space Management
Space management refers to how a file system allocates and manages disk space. It includes:
- **Allocation Methods**: Techniques like contiguous, linked, and indexed allocation.
- **Free Space Management**: Methods to track free space, such as bitmaps or free lists.
- **Fragmentation**: Handling and minimizing fragmentation to optimize space usage.

#### 2. Filename
The filename is the human-readable identifier for a file. Key aspects include:
- **Naming Conventions**: Rules for valid characters, length, and case sensitivity.
- **Extensions**: Suffixes that indicate file types (e.g., `.txt`, `.jpg`).
- **Uniqueness**: Ensuring filenames are unique within a directory.

#### 3. Directory
A directory is a structure that organizes and provides access to files. Important points are:
- **Hierarchy**: The directory tree structure, including root and subdirectories.
- **Pathnames**: Absolute and relative paths to locate files.
- **Operations**: Creating, deleting, and navigating directories.

#### 4. Metadata
Metadata is data about data, providing information about files. It includes:
- **Attributes**: File size, creation/modification dates, permissions, and ownership.
- **Access Control**: Security settings to control who can read, write, or execute the file.
- **File Type**: Information about the file format and type.

#### 5. Utilities
Utilities are tools provided by the file system for managing files and directories. Examples include:
- **File Operations**: Commands like copy, move, delete, and rename.
- **Disk Utilities**: Tools for disk checking, defragmentation, and backup.
- **Search Tools**: Utilities to find files based on various criteria.

#### 6. Design
The design of a file system encompasses its overall architecture and features. Key considerations are:
- **Performance**: Efficiency in file access and management.
- **Reliability**: Ensuring data integrity and fault tolerance.
- **Scalability**: Ability to handle growing amounts of data and users.
- **Compatibility**: Support for different operating systems and hardware.

### Types of Linux File System

#### Some Important terms:
  1. **Journaling:** Journaling file systems keep a log called the `journal`, that keeps track of the changes made to a file but not yet permanently committed to the disk so that in case of a system failure the lost changes can be brought back.
  2. **Versioning:** Versioning file systems store previously saved versions of a file, i.e., the copies of a file are stored based on previous commits to the disk in a minutely or hourly manner to create a backup.
  3. **Inode:** In Linux, everything is a file i.e., to uniquely identify each and every file, it generates a unique number reffered as `inode number`. This inode number refers to the metadata of a perticular file which mainly includes size, permissions, ownership, when last time is is modified? When it is created? etc.

![Types of Linux File System](https://media.geeksforgeeks.org/wp-content/uploads/20231128123434/Types-of-Linux-File-System.png)

> [!NOTE]
> Cluster and distributed file systems will not be included for simplicity.

1) ext (Extended File System): 
Implemented in 1992, it is the first file system specifically designed for Linux. It is the first member of the ext family of file systems.

2) ext2: 
The second ext was developed in 1993. It is a non-journaling file system that is preferred to be used with flash drives and SSDs. It solved the problems of separate timestamp for access, inode modification and data modification. Due to not being journaled, it is slow to load at boot time.

3) Xiafs: 
Also developed in 1993, this file system was less powerful and functional than ext2 and is no longer in use anywhere.

4) ext3: 
The third ext developed in 1999 is a journaling file system. It is reliable and unlike ext2, it prevents long delays at system boot if the file system is in an inconsistent state after an unclean shutdown. Other factors that make it better and different than ext2 are online file system growth and HTree indexing for large directories.

5) JFS (Journaled File System):
First created by IBM in 1990, the original JFS was taken to open source to be implemented for Linux in 1999. JFS performs well under different kinds of load but is not commonly used anymore due to the release of ext4 in 2006 which gives better performance.

6) ReiserFS: 
It is a journal file system developed in 2001. Despite its earlier issues, it has tail packing as a scheme to reduce internal fragmentation. It uses a B+ Tree that gives less than linear time in directory lookups and updates. It was the default file system in SUSE Linux till version 6.4, until switching to ext3 in 2006 for version 10.2.

7) XFS: 
XFS is a 64-bit journaling file system and was ported to Linux in 2001. It now acts as the default file system for many Linux distributions. It provides features like snapshots, online defragmentation, sparse files, variable block sizes, and excellent capacity. It also excels at parallel I/O operations.

8) SquashFS: 
Developed in 2002, this file system is read-only and is used only with embedded systems where low overhead is needed.

9) Reiser4: 
It is an incremental model to ReiserFS. It was developed in 2004. However, it is not widely adapted or supported on many Linux distributions.

10)  ext4: 
The fourth ext developed in 2006, is a journaling file system. It has backward compatibility with ext3 and ext2 and it provides several other features, some of which are persistent pre-allocation, unlimited number of subdirectories, metadata checksumming and large file size. ext4 is the default file system for many Linux distributions and also has compatibility with Windows and Macintosh.

11)  btrfs (Better/Butter/B-tree FS): 
It was developed in 2007. It provides many features such as snapshotting, drive pooling, data scrubbing, self-healing and online defragmentation. It is the default file system for Fedora Workstation.

12)  bcachefs: 
This is a copy-on-write file system that was first announced in 2015 with the goal of performing better than btrfs and ext4. Its features include full filesystem encryption, native compression, snapshots, and 64-bit check summing.

13)  Others:
 Linux also has support for file systems of operating systems such as NTFS and exFAT, but these do not support standard Unix permission settings. They are mostly used for interoperability with other operating systems.

## 3.2 Hierarchy of Directories
Linux follows a hierarchical directory structure in which all the files are grouped together and organized in well sutaible manner.

![Hierarchy of Directories](https://i.redd.it/j2k4k1j6f1x71.jpg)

### /bin (binaries)
/bin is the directory that contains binaries, that is, some of the applications and programs you can run. You will find the ls program mentioned above in this directory, as well as other basic tools for making and removing files and directories, moving them around, and so on. There are more bin directories in other parts of the file system tree, but we’ll be talking about those in a minute.

### /boot
The /boot directory contains files required for starting your system. It consist of bootloader files, kernel, other booting related files. 

> [!WARNING] Do I have to say this?
> Okay, I’ll say it: DO NOT TOUCH!. If you mess up one of the files in here, you may not be able to run your Linux and it is a pain to repair. 
> 
> On the other hand, don’t worry too much about destroying your system by accident: you have to have superuser privileges to do that.

### /dev (device)
/dev contains device files. Many of these are generated at boot time or even on the fly. For example, if you plug in a new webcam or a USB pendrive into your machine, a new device entry will automagically pop up here.

### /etc
/etc is the directory where names start to get confusing. /etc gets its name from the earliest Unixes and it was literally “et cetera” because it was the dumping ground for system files administrators were not sure where else to put.

Nowadays, it would be more appropriate to say that etc stands for “Everything to configure,” as it contains most, if not all system-wide configuration files. For example, the files that contain the name of your system, the users and their passwords, the names of machines on your network and when and where the partitions on your hard disks should be mounted are all in here. Again, if you are new to Linux, it may be best if you don’t touch too much in here until you have a better understanding of how things work.

### /home
/home is where you will find your users’ personal directories. In my case, under /home there are two directories: /home/paul, which contains all my stuff; and /home/guest, in case anybody needs to borrow my computer.

### /lib (libraries)
/lib is where libraries live. Libraries are files containing code that your applications can use. They contain snippets of code that applications use to draw windows on your desktop, control peripherals, or send files to your hard disk.

There are more lib directories scattered around the file system, but this one, the one hanging directly off of / is special in that, among other things, it contains the all-important kernel modules. The kernel modules are drivers that make things like your video card, sound card, WiFi, printer, and so on, work.

### /media
The /media directory is where external storage will be automatically mounted when you plug it in and try to access it. As opposed to most of the other items on this list, /media does not hail back to 1970s, mainly because inserting and detecting storage (pendrives, USB hard disks, SD cards, external SSDs, etc) on the fly, while a computer is running, is a relatively new thing.

### /mnt (mount)
The /mnt directory, however, is a bit of remnant from days gone by. This is where you would manually mount storage devices or partitions. It is not used very often nowadays.

### /opt (optional)
The /opt directory is often where software you compile (that is, you build yourself from source code and do not install from your distribution repositories) sometimes lands. Applications will end up in the /opt/bin directory and libraries in the /opt/lib directory.

A slight digression: another place where applications and libraries end up in is /usr/local, When software gets installed here, there will also be /usr/local/bin and /usr/local/lib directories. What determines which software goes where is how the developers have configured the files that control the compilation and installation process.

### /proc (proccess)
/proc, like /dev is a virtual directory. It contains information about your computer, such as information about your CPU and the kernel your Linux system is running. As with /dev, the files and directories are generated when your computer starts, or on the fly, as your system is running and things change.

### /root
/root is the home directory of the superuser (also known as the “Administrator”) of the system. It is separate from the rest of the users’ home directories BECAUSE YOU ARE NOT MEANT TO TOUCH IT. Keep your own stuff in your own directories, people.

### /run
/run is another new directory. System processes use it to store temporary data for their own nefarious reasons. This is another one of those DO NOT TOUCH folders.

### /sbin (system binaries)
/sbin is similar to /bin, but it contains applications that only the superuser (hence the initial s) will need. You can use these applications with the sudo command that temporarily concedes you superuser powers on many distributions. /sbin typically contains tools that can install stuff, delete stuff and format stuff. As you can imagine, some of these instructions are lethal if you use them improperly, so handle with care.

### /usr (user)
The /usr directory was where users’ home directories were originally kept back in the early days of UNIX. However, now /home is where users kept their stuff as we saw above. These days, /usr contains a mish-mash of directories which in turn contain applications, libraries, documentation, wallpapers, icons and a long list of other stuff that need to be shared by applications and services.

You will also find bin, sbin and lib directories in /usr. What is the difference with their root-hanging cousins? Not much nowadays. Originally, the /bin directory (hanging off of root) would contain very basic commands, like ls, mv and rm; the kind of commands that would come pre-installed in all UNIX/Linux installations, the bare minimum to run and maintain a system. /usr/bin on the other hand would contain stuff the users would install and run to use the system as a work station, things like word processors, web browsers, and other apps.

But many modern Linux distributions just put everything into /usr/bin and have /bin point to /usr/bin just in case erasing it completely would break something. So, while Debian, Ubuntu and Mint still keep /bin and /usr/bin (and /sbin and /usr/sbin) separate; others, like Arch and its derivatives just have one “real” directory for binaries, /usr/bin, and the rest or *bins are “fake” directories that point to /usr/bin.

### /srv (services)
The /srv directory contains data for servers. If you are running a web server from your Linux box, your HTML files for your sites would go into /srv/http (or /srv/www). If you were running an FTP server, your files would go into /srv/ftp.

### /sys (system)
/sys is another virtual directory like /proc and /dev and also contains information from devices connected to your computer.

In some cases you can also manipulate those devices. I can, for example, change the brightness of the screen of my laptop by modifying the value stored in the /sys/devices/pci0000:00/0000:00:02.0/drm/card1/card1-eDP-1/intel_backlight/brightness file (on your machine you will probably have a different file). 

> [!CAUTION]
But to do that you have to become superuser. The reason for that is, as with so many other virtual directories, messing with the contents and files in `/sys` can be dangerous and you can trash your system. DO NOT TOUCH until you are sure you know what you are doing.

### /tmp (temparary)
/tmp contains temporary files, usually placed there by applications that you are running. The files and directories often (not always) contain data that an application doesn’t need right now, but may need later on.

You can also use /tmp to store your own temporary files — /tmp is one of the few directories hanging off / that you can actually interact with without becoming superuser.

### /var (variable)
/var was originally given its name because its contents was deemed variable, in that it changed frequently. Today it is a bit of a misnomer because there are many other directories that also contain data that changes frequently, especially the virtual directories we saw above.

Be that as it may, /var contains things like logs in the /var/log subdirectories. Logs are files that register events that happen on the system. If something fails in the kernel, it will be logged in a file in /var/log; if someone tries to break into your computer from outside, your firewall will also log the attempt here. It also contains spools for tasks. These “tasks” can be the jobs you send to a shared printer when you have to wait because another user is printing a long document, or mail that is waiting to be delivered to users on the system.

> [!NOTE]
> Different Linux flavors/distributions may have a slightly different hierarchy. For example, some new directories may be present.
>
> One example is the `/srv` directory, which is used in some Linux distributions like Debian and Ubuntu to store data for services provided by the system, such as web servers. This directory might not be present or used in the same way in other distributions like Arch Linux or Fedora.

### To explore the filesystem yourself, use the following commands:

- **cd (change directory):** will take you to the directory of your choice. You can move around with different different directories.

    ```sh
    $ cd <path of the directory>
    ```

> [!NOTE]
> Linux provides you two special directories in each directory which are dot `.` and double dots `..` 
>
> The `.` refers to the current working directory & `..` directory refers to the previous (parent) directory.
>
> ```sh
> $ cd ..   // Goes one directory back

- **pwd (print working directory):** If you forgot, where you are currently working, this command will print the current working directory you are working on.

    ```sh
    $ pwd
    /home/csiber/Documents
    ```

- **ls (list):** This command display the list of all the files present in the perticular directory. It accepts an optional argument i.e., path of the directory. If the path is provided it will show the list of files present in the given path, else if path is not provided, then lists the files in the current working directory

    ```sh
    $ ls    // Displays list of files in the current working directory

    $ ls <path of directory>    // Displays list of files in the provided path
    ```

## 3.3 Types of Files in Linux

In Linux, the filesystem categorizes files into different types based on their functionality and characteristics. Here's an overview of the major file types in Linux:

### 1) **Regular / Ordinary Files (-)**
Regular files are the most common type of files in Linux. They contain user data such as text, program instructions, or media files. Regular files can be readable, writable, or executable, depending on the permissions set. Examples include text documents, images, scripts, and binary executable files. These files do not have any special attributes other than storing data.

You can list all the regualar files using the find command:
```bash
$ find /path/to/directory -type f
```

### 2) **Directory Files (d)**
A directory is a special type of file that contains references to other files and subdirectories. Directories are used to organize files into a hierarchical structure. Each directory holds file names and pointers to their respective locations on the disk. 

In Linux, directories can be navigated using commands like `cd`, and the contents can be listed using `ls`. The root directory ("/") is the base of the filesystem, and every other directory is a child of it.

You can list all the directories using the find command:
```bash
$ find /path/to/directory -type d
```

### 3) **Special Files**
Linux treats all hardware devices (such as hard drives, printers, monitors, terminal emulators, and CD/DVD drives) as special files. Applications access and use device files as regular files. This feature makes software development in Linux straightforward and flexible. Linux places all device files under the /dev directory. There are two types of device files: character and block. A character file represents a device that transfers data in bytes, such as a monitor or a printer. A block file represents a device that transfers data in blocks, such as a hard drive.

Special files represent devices or other resources and provide an interface to interact with hardware or system components. These files are found in the `/dev` directory and include several subtypes:

#### a) **Block Files (b)**
Block special files provide buffered access to hardware devices that read or write data in fixed-sized blocks. These files represent storage devices like hard drives, SSDs, and USB drives. Block devices allow random access to data, meaning you can read and write specific blocks independently. Common block device files include `/dev/sda` (a hard drive).

You can list all the block files using the find command:
```bash
$ find /path/to/directory -type b
```

#### b) **Character Files (c)**
Character special files provide unbuffered access to devices and handle data as a stream of bytes, one character at a time. They represent devices like keyboards, mice, serial ports, and terminals. Since these devices handle continuous input/output, they are accessed sequentially. A common character file would be `/dev/tty` (terminal).

You can list all the character files using the find command:
```bash
$ find /path/to/directory -type c
```

#### c) **Named Pipe Files (p)**
It is a special type of file that acts as a communication channel between different processes, also known as a FIFO (First In, First Out), allowing data to be passed from one process to another in a sequential manner, essentially acting like a virtual pipe within the file system that can be accessed by its name unlike a regular unnamed pipe which only lasts as long as the creating process does; you can create named pipes using the `mkfifo` command.

You can list all the named pipe files using the find command:
```bash
$ find /path/to/directory -type p
```

#### d) **Links (l)**
Links are pointers or references to other files and are used to manage file redundancy and efficient storage. There are two types of links:

**i) Soft Links (Symbolic Links):**  
  A soft link is a reference or shortcut to another file, similar to a Windows shortcut. It points to the file’s pathname. If the original file is deleted, the soft link becomes invalid or "broken." It can link to directories and even files across different filesystems.

**ii) Hard Links:**  
  A hard link points to the actual inode of a file, essentially creating another name for the same file. Unlike soft links, hard links remain valid even if the original file is deleted, as long as one hard link exists. Hard links cannot span across different filesystems or link to directories.

You can list all the links using the find command:
```bash
$ find /path/to/directory -type l
```

#### e) **Socket Files (s)**
Socket files enable inter-process communication (IPC) over a network or within the same system. These files allow communication between different processes or systems by exchanging data through a file-like interface. They are used for network services and represent endpoints for communication between processes. Socket files can be found in `/var/run` or `/tmp`.

You can list all the socket files using the find command:
```bash
$ find /path/to/directory -type s
```

### Conclusion:

![](https://www.tecmint.com/wp-content/uploads/2016/05/Everything-is-a-File-in-Linux.png)

## 3.4 File Permissions and Ownership
As everything is a file in Linux, there is a one who created the file is known as the `owner` of that file. The permissions are devided into following three parts: 

1. **User (u):** Which is also known as the owner of the file.
2. **Group (g):** Whenever the user creates a file s/he will become the owner of that file and the group s/he belongs to will become the default group of that file. The permissions assigned to the group will be automatically applied to all the group members.
3. **Other (o):** These permissions refers to all the user accounts except the owner of the file.

> [!IMPORTANT]
> Don't confuse you're self about the `u` and `o` notation. Since `o` quickly reminds us the `owner` but it is not. So make sure to remember the pattern `ugo`.

### Types of permissions
Commonly, there are three types of permissions present in any linux/unix based Operating System. These are:

1. **read (r):** Allows users to view the contents of a file or list the files within a directory
2. **write (w):** Allows the users to modify the contents of a file or create, delete, and rename files within a directory.
3. **execute (x):** Allows the users to run a file (if it's a program) or access a directory.

> [!NOTE]
> Linux is often regarded as one of the most secure operating systems in the world. By default, it does not allow any file to execute without explicit permission, requiring users to manually manage executable permissions.
>
> Additionally, Linux isolates potential threats, preventing viruses from spreading throughout the entire system.

### How to check the permissions?
You can check the permissions of a specific file or directory using the using the `stat` command followed by the name of the file (or directory) or path and it will show you all the metadata of the file/directory with permissions

```bash
$ stat content.png 
  File: content.png
  Size: 960315          Blocks: 1880       IO Block: 4096   regular file
Device: 8,18    Inode: 12390631    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/csiber)   Gid: ( 1000/csiber)
Access: 2025-02-08 10:14:04.076904616 +0530
Modify: 2025-02-03 11:36:50.115622139 +0530
Change: 2025-02-03 11:36:50.115622139 +0530
 Birth: 2025-02-03 11:36:48.803615791 +0530
```

OR you can also long list the directory to list the files and directories with there respective permissions

```bash
$ ls -l
total 151756
drwxrwxr-x  3 csiber csiber      4096 Sep 30 20:15  Android
drwxr-xr-x  3 csiber csiber      4096 Feb 14 18:21  Desktop
drwxr-xr-x  5 csiber csiber      4096 Jan  1 14:54  Documents
drwxr-xr-x  6 csiber csiber      4096 Feb 17 11:36  Downloads
drwxr-xr-x  2 csiber csiber      4096 Oct 11 12:51  Music
drwxr-xr-x  3 csiber csiber      4096 Feb 11 08:36  Pictures
drwxr-xr-x  2 csiber csiber      4096 Jun 23  2024  Public
drwxr-xr-x  3 csiber csiber      4096 Dec 13 12:26  Videos
```

### How to read permissions?

To read the permissions in Linux, you need to understand the format of the permission string. Let's break down the example `drwxrw-rw-`:

1. **File Type:**
    - The first character indicates the file type:
      - `d` for directory
      - `-` for regular file
      - `l` for symbolic link
      - `c` for character device
      - `b` for block device
      - `p` for named pipe (FIFO)
      - `s` for socket

2. **User (Owner) Permissions:**
    - The next three characters represent the owner's permissions:
      - `r` for read
      - `w` for write
      - `x` for execute
      - `-` if the permission is not granted

    In this example, `rwx` means the owner has read, write, and execute permissions.

3. **Group Permissions:**
    - The following three characters represent the group's permissions:
      - `r` for read
      - `w` for write
      - `x` for execute
      - `-` if the permission is not granted

    In this example, `rw-` means the group has read and write permissions, but not execute.

4. **Other (World) Permissions:**
    - The last three characters represent the permissions for others (everyone else):
      - `r` for read
      - `w` for write
      - `x` for execute
      - `-` if the permission is not granted

    In this example, `rw-` means others have read and write permissions, but not execute.

So, `drwxrw-rw-` indicates a directory where the owner has full permissions (read, write, execute), the group has read and write permissions, and others have read and write permissions.

![Linux File System](https://i.sstatic.net/JKUg3.jpg)

### Assigning Permissions in Linux

In Linux, permissions control who can access and modify files and directories. There are two main ways to assign permissions:

### 1. Absolute Method (Octal Representation)

This method uses numbers to represent permissions. Each permission (read, write, execute) is assigned a value:

* Read (r) = 4
* Write (w) = 2
* Execute (x) = 1

These values are combined to represent permissions for the owner, group, and others. For example, 7 (4+2+1) means read, write, and execute permission.

**Example:**

To give the owner read, write, and execute permission (7), the group read and execute permission (5), and others read and execute permission (5), you would use the command:

```bash
$ chmod 755 myfile.txt
```

### 2. Relative Method (Symbolic Representation)

This method uses letters to represent permissions and operators to modify them.

* `r` for read, `w` for write, `x` for execute
* `u` for owner, `g` for group, `o` for others, `a` for all
* `+` to add permission, `-` to remove permission, `=` to set permission

**Example:**

To add write permission for the group to a file, you would use the command:

```bash
$ chmod g+w myfile.txt
```

To remove execute permission for others from a file, you would use the command:

```bash
$ chmod o-x myfile.txt
```

#### Octal Representation and rwx Conversion

Octal representation uses three bits to represent permissions for each category (owner, group, others). Each bit corresponds to a permission:

* Read (r) - 4
* Write (w) - 2
* Execute (x) - 1

The combination of these bits forms a number from 0 to 7. Here's a table showing the combinations and their octal representation:

| Octal | Binary | rwx  |
|:-----:|:------:|:----:|
| 0     | 000    | ---  |
| 1     | 001    | --x  |
| 2     | 010    | -w-  |
| 3     | 011    | -wx  |
| 4     | 100    | r--  |
| 5     | 101    | r-x  |
| 6     | 110    | rw-  |
| 7     | 111    | rwx  |

For example, the octal number 6 (110 in binary) represents read and write permissions (r and w).

In summary, the absolute method uses numbers to set permissions directly, while the relative method uses letters and operators to modify existing permissions. Both methods are useful in different situations.

> [!TIP]
> For beginners in Linux, it's recommended to use the relative method for changing permissions. This method only adds or removes specific permissions for a specific type of user (user, group, other). Using the octal representation can be confusing and may accidentally assign extra permissions, leading to potential security issues. Stick with the relative method to avoid headaches! and don't use the absolute method until and unless you know what you are doing!

## 3.5 Mounting & Unmounting File Systems
In Linux/Unix, mounting and unmounting file systems are essential tasks for accessing and managing storage devices. Mounting a file system attaches it to a specific directory within the existing file hierarchy, making its contents accessible. Unmounting detaches the file system, making it inaccessible until it is mounted again.

### Mounting a File System

To mount a file system, you use the `mount` command followed by the device and the directory where you want to mount it (the mount point).

**Syntax:**
```sh
$ mount [options] device mount_point
```

**Example: Mounting a Pendrive**

1. **Insert the Pendrive:** Plug in the pendrive to your system.
2. **Identify the Device:** Use the `lsblk` or `fdisk -l` command to identify the device name (e.g., `/dev/sdb1`).

    ```sh
    $ lsblk
    ```

3. **Create a Mount Point:** Create a directory where you want to mount the pendrive.

    ```sh
    $ sudo mkdir /mnt/pendrive
    ```

4. **Mount the Pendrive:** Use the `mount` command to mount the pendrive.

    ```sh
    $ sudo mount /dev/sdb1 /mnt/pendrive
    ```

5. **Access the Pendrive:** You can now access the contents of the pendrive at `/mnt/pendrive`.

### Unmounting a File System

To unmount a file system, you use the `umount` command followed by the device or the mount point.

**Syntax:**
```sh
$ umount [options] device_or_mount_point
```

**Example: Unmounting a Pendrive**

1. **Unmount the Pendrive:** Use the `umount` command to unmount the pendrive.

    ```sh
    $ sudo umount /mnt/pendrive
    ```

2. **Remove the Mount Point (Optional):** If you no longer need the mount point, you can remove it.

    ```sh
    $ sudo rmdir /mnt/pendrive
    ```

### Listing All File Systems

To list all the mounted file systems, you can use the `df` or `mount` command.

**Example: Using `df` Command**

```sh
$ df -h

Filesystem      Size  Used Avail Use% Mounted on
udev            3.8G     0  3.8G   0% /dev
tmpfs           787M  1.6M  785M   1% /run
/dev/sdb2       210G  123G   77G  62% /
tmpfs           3.9G   78M  3.8G   2% /dev/shm
efivarfs        100K   87K  8.4K  92% /sys/firmware/efi/efivars
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-udev-load-credentials.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-sysctl.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup-dev-early.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup-dev.service
tmpfs           3.9G   14M  3.9G   1% /tmp
/dev/sdb1       511M  152K  511M   1% /boot/efi
/dev/loop2       39M   39M     0 100% /snap/snapd/21759
/dev/loop1       84M   84M     0 100% /snap/scrcpy/399
/dev/loop0       56M   56M     0 100% /snap/core18/2829
/dev/sda3       245G  103G  142G  43% /media/csiber/sdb3
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
tmpfs           787M  160K  787M   1% /run/user/1000
```

**Example: Using `mount` Command**

```sh
$ mount | column -t
```

These commands will display information about all the mounted file systems, including their mount points, device names, and usage statistics.

## Conclusion

In this chapter, we explored various aspects of file systems and their management in Linux. We started by understanding the different types of file systems and their purposes. We then delved into mounting and unmounting file systems, learning how to use the `mount` and `umount` commands effectively. We also covered how to check disk usage and file system statistics using commands like `df` and `du`.

Additionally, we examined the use of `tmpfs` for temporary file storage and how to view mounted file systems with the `mount` command. By the end of this chapter, you should have a solid understanding of how to manage and monitor file systems in a Linux environment.

Keep practicing these commands and concepts to become proficient in handling file systems on your own.