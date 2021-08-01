# Linux Basics for Hackers - Notes

## The basics

`pwd` - print current working directory

`whoami` - check which user is logged in

`cd ..` - move up 1 level in the file structure

`cd /`  - move up to the root level

`ls` - list

`ls -l` - long listing (whether an object is a file or directory, the number of links, the
owner, the group, its size, when it was created or modified, and its name.)

`ls -la` - show hidden files

double dash (- -) before word options, ex: - - help

single dash (-) before single-letter options, ex: -h

`man` - referencing manual pages, ex: `man aircrack-ng`. `q` for quit.

`locate`- locate every occurrence of that word, ex: `locate aircrack-ng`. (disadvantage: the database updates only once a day)

`whereis` - locate binary files and possibly their *man* page.

`which` - returns the location of the binaries in the PATH variable in Linux. Ex: `which aircrack-ng`

`find` *directory options expression*. ex: `find / -type f -name apache2` means search **apache** in the **root** directory and looking for the type of file that is an **ordinary file**. find displays only *exact* name matches. We can use wildcards: `find / -type f -name apache2.*` looking for any extension after the filename *apache2*

Filtering with **grep**

- piping - take the output of one command and send it as input to another command.

`ps aux` - displays all processes running in the system.

`ps aux | grep apache2` - check if the apache2 service is running.

`cat` - display the contents of that file. Ex: `cat test.txt`; `cat > newfile` - creates a new file called *newfile*. Start typing the contents then press CTRL-D to save and exit.

`cat >> filename` - append content to the file.

`touch newfile` - creates a new file

`mkdir newdirectory`- create a new directory

`cp oldfile /root/newdirectory/newfile` - copy the **oldfile** to the /root/newdirectory and rename it as **newfile**

Renaming a file - `mv oldfile newfile`

Remove a file - `rm filename`

Remove a directory - `rmdir directoryname` - if the directory is not empty, the directory will not be removed. Use `rm -r directoryname` to remove a directory and its content all in one go.

## Text Manipulation

Display the first 10 lines of a file - `head filename`

Display the first n lines of a file - `head -n filename`

Display the last 10 lines of a file - `tail filename`

Display the last n lines of a file - `tail -n filename`

**Numbering the lines** - `nl filename`

Filtering text with **grep** - `cat filename | grep output` - display lines with *output* in the filename.

**Find & Replace** - `sed s/mysql/MySQL/g /etc/snort/snort.conf > snort2.conf` - `sed` (stream edit); `s` (search); `g` (replace globally); It means that `mysql` in the `snort.conf` file is replaced by `MySQL` and the result is saved in a new file: `snort2.conf`.

`more` - displays a page of a file at a time. Enter to go on.

`less` - similar to `more` but with more functions. Can search a term’s occurrence by  `/` plus that keyword. `n` to see the next occurrence.

## Network Analysis & Management

`ifconfig`

`iwconfig` - check wireless network devices

Change IP address - `ifconfig eth0 192.168.181.115`

Change Network Mask & Broadcast Address - `ifconfig eth0 192.168.181.115 netmask 255.255.0.0 broadcast 192.168.1.255`

Change MAC address:

- `ifconfig eth0 down`
- `ifconfig eth0 hw ether 00:11:22:33:44:55`
- `ifconfig eth0 up`

Examine DNS with `dig` - `dig google.com nx` - `nx` stands for name server. `mx` option is for *mail exchange server*.

DNS server is stored in `/etc/resolv.conf`

- If you’re using a DHCP address and the DHCP server provides a DNS setting, the DHCP server will replace the contents of the file when it renews the DHCP address.

Mapping your own IP addresses with `/etc/hosts` file

## Adding & Removing Software

apt (Advanced Packaging Tool)

Search for a package - `apt-cache search packageName`

Adding Software - `apt-get istall packageName`

Removing Software - `apt-get remove packageName` - Notice that the configuration file is not removed.

Removing Software together with its config file - `apt-get purge snort`

Update - update the list of packages available for download from the repository - `apt-get update`

Upgrade - upgrade the package to the latest version in the repository - `apt-get upgrade`

Adding repositories to `sources.list` file from `/etc/apt/`

`git clone`

## File & Directory Permissions

Each new user must be added to a group in order to inherit the permissions of that group.

Each file & directory must be allocated a particular level of permission.

- Read
- Write
- Execute (but not necessarily view or edit it)

Granting ownership to an individual user - `chown bob /tmp/bobsfile`

Granting ownership to a group - ` chgrp groupName fileName`

`ls -l` - long listing

- Ex: `drwxr-xr-x` 
  - The first character indicates the file type (`d` for directory; `-` for file)
  - Three sets (**owner**; **group**; **all other users**) of three characters (r, w, x)
  - If any r, w, or x is replaced with a dash (-), then the respective permission hasn’t been given

Changing permissions - `chmod` - change mode

- ON (1); OFF (0). So if `rwx` are all granted, this equates to **111**.

| binary | Octal | r w x |
| ------ | ----- | ----- |
| 000    | 0     | - - - |
| 001    | 1     | - - x |
| 010    | 2     | - w - |
| 011    | 3     | - w x |
| 100    | 4     | r - - |
| 101    | 5     | r - x |
| 110    | 6     | r w - |
| 111    | 7     | r w x |

- Ex: all permissions for the **owner**, **group**, and **all users** - 7 7 7
- Ex: `chmod 774 filename` 

Changing Permissions with *UGO* (user, group, and others)

- `-` removes a permission
- `+` adds a permission
- `=` sets a permission
  - Ex: `chmod u-w filename` - remove (-) the write (w) permission from filename for the user (u).
  - Ex: `chmod u+x,o+x filname` - add execute permission for the user and the other users only. (It seems that there must not be a space between `,` and `o+x` )

Setting more secure default permissions with **Masks**

- `umask` - unmask (3-digit decimal number corresponding to the 3 permissions digits)

- | New files | New directories |                        |
  | --------- | --------------- | ---------------------- |
  | 666       | 777             | Linux base permissions |
  | - 022     | - 022           | `umask`                |
  | 644       | 755             | resulting permissions  |

  - In Kali, and most Debian systems, the `umask` is preconfigured to 022, meaning the Kali default is 644 for files and 755 for directories.
  - `umask` can be changed at `/home/username/.profile`

Special Permissions

- Granting temporary root permissions with **SUID** (set user ID)
  - `chmod 4644 filename` - type a 4 before the regular permissions.
- Granting the Root User’s Group Permissions **SGID** (set group ID)
  - `chmod 2644 filename` - type a 2 before the regular permissions.

## Process Management

### Viewing Processes:

- `ps` - the Linux *kernel* assigns a unique **process ID (PID)** to each process sequentially.

- `ps aux` - shows *all* processes running on the system for *all users*.

Filtering by Process Name - `ps aux | grep programName`

Finding the Greediest Processes with `top` - processes that use the most resources; refresh every 10 seconds. `h` or `?` for more commands; `q` for quit.

### Changing Process Priority with `nice` 

- “how nice you’ll be to other users.”
- Values of `nice` range from -20 to +19; default is 0.
- ![image-20210801152410475](C:\Users\chiatsekuo\AppData\Roaming\Typora\typora-user-images\image-20210801152410475.png)

- Ex: `nice -n -10 /bin/slowprocess` - increment the `nice` value by -10. Meaning increasing its priority and allocating more resources.
- Ex: `nice -n 10 /bin/fastprocess` - increment the `nice` value by 10. Meaning giving the `fastprocess` a lower priority.

### Changing Process Priority with `renice`:

- Values of `renice` range from -20 to +19
- It takes *PID* instead of process name - Ex: `renice 19 6996` - 6996 is the PID
- As with `nice`, only the root user can `renice` a process to a negative value to give it higher priority, but any user can be nice and reduce priority with `renice`.
- Can also use  the`top` utility to change the `nice` value by pressing `r` with the PID and the `nice` value.

### Killing Process

`kill-signal PID` - signal flag is optional. It defaults to SIGTERM.

|         |      |                                                              |
| ------- | ---- | ------------------------------------------------------------ |
| SIGHUP  | 1    | Hang-up (HUP). It stops the process and restarts it with the same PID |
| SIGINT  | 2    | Interrupt (INT). (a weak kill signal that isn’t guaranteed to work) |
| SIGQUIT | 3    | *core dump*. It terminates the process and saves the process in the current working directory to a file named *core*. |
| SIGTERM | 15   | *Termination*. It is the `kill` command’s default kill signal. |
| SIGKILL | 9    | Absolute kill. It forces the process to stop by sending the process’s resources to a special device, `/dev/null`. |

- Ex: `kill -1 6996` or `kill -9 6996`
- `killall -9 zombieprocess` - if we don’t know the process’s ID, just enter the process name.

### Running Processes in the Background

- Ex: `leafpad newscript &` - Now, when the text editor opens, the terminal returns a new command prompt so we can enter other commands while also editing the *newscript*.

### Moving a Process to the foreground

`fg 1234` - `fg` means foreground and `1234` means PID.

### Scheduling Processes

- `at` - scheduling a job to run once at some point in the future
- `crond` - scheduling tasks to occur every day, week, or month.

| Time format          |
| -------------------- |
| at 7:20pm            |
| at 7:20pm June 25    |
| at noon              |
| at noon June 25      |
| at tomorrow          |
| at now + 20 minutes  |
| at now + 10 hours    |
| at now + 5 days      |
| at now + 3 weeks     |
| at 7:20pm 06/25/2019 |

- Ex: `at 7:20am`, then `at` goes into interactive mode. Now enter the command to be executed at that specific time.

## MANAGING USER ENVIRONMENT VARIABLES



left at p.119 MANAGING USER ENVIRONMENT VARIABLES

