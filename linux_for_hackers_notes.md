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

`find` *directory options expression*. ex: `find / -type f -name apache2` means search **apache** in the **root** directory and looking for the type of file that is an **ordinary file**. find displays only *exact* name matches. Therefore, we can use wildcards: `find / -type f -name apache2.*` looking for any extension after the filename *apache2*

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



left at p.82 Software

