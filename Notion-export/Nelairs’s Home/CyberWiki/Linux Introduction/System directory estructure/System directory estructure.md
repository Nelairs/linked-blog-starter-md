So Linux is based on files, everything is a file, everything is a directory

![[Untitled 552.png|Untitled 552.png]]

If you list the root, you’ll find every directory related to  
  

![[Untitled 1 18.png|Untitled 1 18.png]]

Let’s try to focus on the more important ones

- BIN
- BOOT
- DEV
- ETC
- HOME
- LIB
- MEDIA
- OPT

- PROC
- ROOT
- SBIN
- SRV
- SYS
- TMP
- USR
- VAR

### BIN

The /BIN directory is normally an static directory that stores all the basic binaries to ensure user-level functions, so only stores only the user binaries. Since the ones needed for administrative jobs are stored in /SBIN

### SBIN

Here you found the binaries needed for administrative jobs managed by admin or root users

### BOOT

This is another static directory which includes all the files used for the startup of the OS. These are used before the **kernel** starts to give instructions to the system

> The KERNEL is the core of the OS

Here in the BOOT directory is also the GRUB, which is the boot manager.

### DEV

This directory includes all the storage devices that are interpreted by the system as a logic volume, this means any HDD, SSD, M.2, partition, USB, etc.

### ETC

In this directory are stored all the configuration files both at system level and software level. Here is where the **passwd** and **shadow** files are located, because there are configuration files.

### HOME

This is the directory of the standard users, this means Documents, Desktop, Video, Music, etc. There are also temp files from software executed as stduser which are used for the configurations of these programs

### LIB & LIB64

Here are all the necesary libraries which are so the binaries can be executed founded in /bin /sbin and the kernel modules. The /lib64 stores the same libraries but for the system with 64bits architechture.

### MEDIA

This directory represents the mounting point for all the logic volumes that are mounted temporaly from USB to Partitions, etc.

### OPT

This directory is like an extension for the /usr directory but with the condition that all the files are read-only which are part of self contained programs and dont follow the standard of storing the different files in different directories

### PROC

This directory contains the information of the processes and apps which are executed in a determinated moment in the system. These are virtual storages

### ROOT

This pretty much the same as the /home directory but only for the root user

### SRV

This directory is for storing files and directories relatives to servers, such as, FTP, SSH, HTTP, etc

### SYS

This like the /proc directory contains virtual files, but in this case relative to the OS. Here the files are distributed by hierarchy.

### TMP

As its name says here are stored all the temporary files (used when we compromise machines),these could be from system elements or user-level apps as Firefox, Chrome,etc.

### USR

The name means User System Resources, currently it stores readable only files and relative to the user utilities including installed software via apt managers

### VAR

This directory stores files with system information, like logs, emails, DBs, cache, etc. It works something like as a registry of the system.