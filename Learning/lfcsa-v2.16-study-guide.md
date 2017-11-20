# Linux Foundation Certified Sys Admin (LFCSA v2.16) - Study Guide

Mon Nov  6 15:45:45 EST 2017


## Essentials

### touch

-   Change time
-   Change modification date
-   Change access date
-   can use strings like '1 year from now'


### sed (stream editor)

-   `sed 's/oldtext/newtext/g' \\[textfile.txt\]`
-   sed -n "p" filename.txt
-   sed -n "1p" filename.txt
-   sed -n "4,7" filename.txt
-   sed -n "3+7p" filename.txt
-   sed -n "4~1" filename.txt
-   sed -n "1 ! p" filename.txt
-   sed 's/\[\[:regex:]]/{1, } filename.txt


### ls

-   ls -l file1 file2 file3
-   ls -l $(tty) same as ls -l /dev/pts/1

        crw--w----. 1 tux tty 136, 1 Dec 27 14:56 /dev/pts/1


### du

-   `du -sch /some/dir`
-   `c` or `--total`: total size
-   `s`: summarize
-   `h`: human-readable


### sort

-   `-h`: human-readable numeric sort (ascending)
-   -r: reverse order


### uniq

-   `-c`: count the uniq number of occurences
-   `-w \[n]` - the first \\[n\] characters in a line


### fmt - format text

-   fmt filename.txt > newfilename.txt


### cut

-   cut -d 'delemiter' -f1 filename.txt
-   -f1-n: this is the column number for the text input


### Searching ...

#### find

-   search with the -name of the "file"
-   -iname: search ignoring case
-   -not: search for every thing execept
-   c: find character devices (dev)
-   -executable; -readable; writeable
-   -perm \[mode]: files permissions are exactly (octal or symbolic) - ie 700, 644, etc.
-   -type l: for symbolic links
-   -type f: file
-   -type d: directory
-   -d, -depth: process the contents of the directory before the directory itself
-   -maxdepth \[level] or mindepth \[level]
-   -size: find the size of files (less than or greater than)

    -   1M, 24000c, 1K, 1G

-   -group \[group-name]: find files with a given group name
-   find  -atime -1:  access less than a day ago
-   -mtime: modification time

Full Command Lines ...

-   find / -user syslog: all files owned by syslog
-   find / -perm 755: find all files with permissions 755
-   find / -type f -name "some-file.txt" -exec ls -lth {} \; - only look for files name "some-file.txt" then execute the "ls" command.
-   sudo find / -type f -name "myTesT.txt" -exec chmod 707 {} \; -exec ls -l {} \; - find the file named "myTesT.txt", change the permissions, then list the file with a long listing.


**Fun fact**

-   The following command will  will you to look a each line individually until complete. I broke my shell doing this by accident ... :)

    -   sudo find / -type f -name "\*.txt" -exec less {} \;

#### locate

-   locate

    -   sudo updatedb
    -   locate filename filename filename ...
    -   must know the exact name, including case, inorder to use locate.


### Data Backups


#### tar & gzip

-   tar -cvf filename_back.tar filename.txt
-   gzip filename.tar.gz filename.tar
-   tar -zcvf filename_back_gzip.tar.gz filename.txt
-   tar -xzvf filename.tar.gz .
-   --exclude=filname.txt: put this at the end of the cammand to add exclusions.
-   -p: will preserve ownership and file permissions
-   tar -tvf: look at the contents of the archive without unpacking
-   tar -rvf file.tar file.txt or dir/file.txt: append a file to an archive.
-   tar -zvcf exclusion.tar.gz mybkup/ --exclude 'mybkup/ssh': this will exclude the ssh directory within the mybkup directory


### File Attributes

-   -rw**s**-xr-x: the "s" signifies a setuid. The file can be executed against other users by the owner of the file, but all others can only execute the file on themselves..
-   setuid is used in a very limieted manor for system level users

    -   chmod u+s

-   **setgid**: any user can access the directory with the permissions of the group owner

    -   Anything under the parent directory will also get the permissions of the group when the **setgid** option (chmod g+s)is specified on the group
    -   Usually when members of a group need access to all files in a directory regarless of who owns the primary group

-   chmod 2755

-   Sticky bits

    -   Denoted by a "t" in the permissions info
    -   Usually set on directories that a "world" write able.
    -   Prevents a user from removing a file that they do not own inspite of the permissions that are set.

-   lsattr: show special attributes if they exist

-   chattr

    -   i: immutable: cannot delete a file, even as root
    -   a: append mode


### Transfer Files Securely

For ssh setup see <a href="https://github.com/captam3rica/gitMyNotes/blob/master/Linux/generating-ssh-keys.md" target="external" titl="ienerating SSH Keys">generating-shh-keys</a>

#### SCP

-   scp file-to-copy user@hostname:/location/to/copy/to
-   scp user@hostname:/location/to/pull/file/from /location/to/pull/file/to


#### SFTP

-   Able to view and see the remote file system before transferring files
-   Look for sftp in /etc/ssh/sshd_config
-   sftp user@hostname \\[press enter\]. Then, enter password
-   get filename /dir/to/put/file
-   lcd - local change directory


### Monitoring Security & Conduct Audits

-   top
-   htop (install after the fact)
-   free: see memory utilzation

    -   cache aka paging: used to speedup i/o if memory space allows

-   df: disk space usage

    -   df -hTi: check inodes - file system reference

        -   Can find and delete empty inodes

-   du: disk utilization

    -   du -sch /dir/\*: summary of space utilized


-   ps : process management

    -   ps -e: list all processes (same as -A)

    -   ps -ef: every process, full format listing
    -   ps -ef | grep -i \[search\] | grep -v \[what you want to omit\]
    -   ps -axjf: running processes in a tree view (similar to ps -ejH)
    -   ps -eLf: see thread information
    -   ps -U \[username] -u \[username] u: see all process owned by a user
    -   ps -aux: see all process regarless of user
    -   ps aux: see all processes (BSD style)
    -   ps au: list all process belonging to a terminal regardless of user (will list username)
    -   ps t \[tty#]: select based on tty#
    -   ps -d: select all but session leaders
    -   ps r: select only the **running** processes
    -   ps -p \[pid]: select based on process id (same as p & --pid, also -\[pid #] ie -1234)

    The use of BSD-style options will add process state **(stat=STAT)** to the default display and show the comma nd args **(args=COMMAND)** instead of the executable name. You can override this with the PS_FORMAT environment variable. The use of BSD-style options will also change the process selection to **include processes on other terminals (TTYs) that are owned by you**; alternately, this may be described as setting the selection to be the set of all processes filtered to exclude processes owned by other users or not on a terminal.

-   log files

    -   dmesg: print or control kernel ring buffer
    -   journalctl: query the systemd journal


## Operating on Running Systems

### Process Priority & Resource Utilization

-   pgrep: find a process id for a process

-   kill \[options] \[pid]

    -   kill -KILL: another way to send a kill signal. Given to the OS to shutdown the process instead of directly to the process. (same as -9)
    -   kill -SIGHUP: hangup the process


-   nice: run process with modified scheduling priority

    -   nice -n \[priority -20 to 20]


-   renice -n \[priority] -g|-p|-u \[id-of-process]: alter the priority of an already running process


### Manage Startup Process and Services

-   upstart: debian and Ubuntu < 14.04

    -   /etc/init/: process configuration files
    -   echo "manual" | tee /etc/init/\[service-name].override: this will put a process in a "stopped" or "waiting" state on boot as long as this file is in place. The name must be exactly as the service name.
    -   status \[process-name]


-   systemd: Ubuntu 14.04 up, RHEL, CentOS, Arch, Fedora ...

    -   Ubuntu

        -   /etc/init
        -   systemctl 16.04

    -   CentOS

        -   /etc/rc.d
        -   /etc/rc.d/init.d: All of the runlevels

    -   /etc/systemd/system: service runlevel targets

    -   --now: when used with "enable", will start the process as well

    -   set-default graphical.target: make the graphical interface the default target

    -   isolate graphical.target: this will cause the graphical interface to start immediately


### Package Management (Debian/Ubuntu)

**NOTE**: If you are a regular user remember to use *sudo* in front of these commands.

Information regarding the .deb repo source list can be found [HERE](https://manpages.debian.org/stretch/apt/sources.list.5.en.html)

-   /etc/apt/sources.list: list of repos

All of the **APTs** man pages can be found [HERE](https://manpages.debian.org/stretch/apt/index.html)

#### apt

**APT** man page [HERE](https://manpages.debian.org/stretch/apt/apt.8.en.html)

-   apt --installed list:

    Show a list of installed packages.


#### apt-cache

[man apt-cache](https://manpages.debian.org/stretch/apt/apt-cache.8.en.html)

-   apt-cache pkgnames: See a list of packages that **APT** knows about.

-   apt-cache show \[package-name]: information about a package

-   apt-cache stats: information about packages in the cache

-   apt-cache depends \[package-name]: show a list of deps for a particular package and the packages that could fulfill those deps.

    -   apt-cache depends --installed \[package-name]: limit the output to deps that are currently installed.


#### apt-get

[man apt-get](https://manpages.debian.org/stretch/apt/apt-get.8.en.html "Manual pages for apt-get")

-   apt-get clean: clean up the apt cache

    -   /var/cache/apt/archives/ and /var/cache/apt/archives/partial/

-   apt-get autoclean: clean any partial  package

    -   /var/cache/apt/archives and /var/cache/apt/archives/partial/

    Like clean, autoclean clears out the local repository of retrieved package files. The difference is that it only removes package files that can no longer be downloaded, and are largely useless

-   apt-get update && apt-get upgrade: to update and install package updates

-   apt-get dist-upgrade: upgrade the distro

    -   will determine which packages are compatible or which are not

-   apt-get autoremove:

    autoremove is used to remove packages that were automatically installed to satisfy dependencies for other packages and are now no longer needed.

-   apt-get remove \[package name]: will remove the package, but not fully

    -   apt-get remove --purge \[package-name]: Completely delete package

    -   apt-get remove --autoremove \[package-name]:

        Like **autoremove**, will uninstall unused deps associated with a given package.

-   apt-get --download-only \[package name] or apt-get download \[package name]

    -   Will download to the pwd
    -   Make sure that all deps are there (the distro will tell what is needed)
    -   dpkg -i \[depfile.deb]

-   apt-get --reinstall \[package-name]: attempt to reinstall a package

-   apt-get changelog \[packag name]

-   apt-get check: let us know which deps may be broken

    -   apt-get build-dep \[package name]: grab and build deps packages for a give package


#### dpkg - install **.deb** packages

The following commands will only work on locally installed packages and files.

    -   dpkg -l: list the installed packages

    -   dpkg -L \[package names]:

        List all of the files and dirs created after a packages is installed.

    -   dpkg -S [packagename.version]:

        Find out which package provides a particular file.  

#### apt-file

**apt-file** is a software package that indexes the contents of packages in your available repositories and allows you to search for a particular file among all available packages. (ABSOLUTELY EVERYTHING!!!!!!!!!!!)

Before using **apt-file** it must be installed & the DB updated

      apt-get install apt-file
      apt-file update


-   apt-file search \[filename or packagename]


#### aptitude is a fronted for dpkg. Similar to ncurses


### Package Management (CentOS & RHEL)

[Tips & Tricks](https://wiki.centos.org/TipsAndTricks/YumAndRPM)

#### yum (yellow dog updater) - from Yellow Dog Linux

Yum cheat sheet [here](/gitMyNotes/Linux/rhel_yum_cheatsheet.pdf)

[man yum](https://www.unix.com/man-page/centos/8/yum/)

-   What should you do the first time you login to a newly installed system - yum check-update && yum update

-   yum search \[package] and yum list | grep \[package] are synonymous

    -   yum search all \[package]: find all results containing the package name.

-   yum provides \[file-name]: look for a package that provides a specific file or feature.

-   yum info \[package]

-   yum updateinfo \[category]: get info about available updates

    -   you udateinfo security: get info about security updates.

-   yum update-to \[package-version]: update to a particular package version

-   yum downgrade \[package-version]: downgrade a package to a previous version

-   yum install --downloadonly --downloaddir=\[/dir/path/] \[package]: download but do not install and put in the specified directory.

-   yum localinstall \[local-rpm-package]: install a local package

-   yum install yum-utils: extra utilities revolving around yum

    -   yumdownloader \[package]: to get the .rpm only
    -   needs-restarting: see which recently updated processes that need to be restarted.

-   yum list | less: see a list of available packages in the yum db, the versions, and the repos that they belong to.

-   yum list installed: see a list of locally installed packages

-   yum grouplist: see a list of group packages

    -   yum groupinstall \['group name']
    -   yum groupupdate
    -   yum groupremove \['group name']

-   yum repolist

    -   yum repolist all
    -   yum --enablerepo=\[repo-name] install \[package-name]

-   yum clean all: removes ... plugins, rpmdb, dbcache, metadata, headers, packages, expire-cache

    -   yum clean packages: clean up cached packages

-   yum history

-   yum remove \[package-name]: will only remove the specified package. It will not remove the deps.

    -   yum erase \[package]: does the same as "yum remove"
    -   yum autoremove \[package]: same as "remove" plus gets rid of unneeded packages (RHEL7)



#### rpm (RedHat Package Manager)

-   rpm -ivh

-   rpm -Uvh: if the package exists, upgrade it. Otherwise, install it.

-   rpm2cpio \[package-and-version] | cpio -ivd /download/location: extract a file from a package

    -   cpio: copy files to and from archives

-   rpm *will not* install deps for a particular package

    -   can use **yum** to get deps
    -   `yum install \[package]`
    -   `yum remove \[package`
    -   yum should have only removed the specified \[package] and left the deps behind to be utilized by rpm or saved for later.

-   rpm -q --changelog \[package] | less: see the change logs for a package

-   rpm -q \[package]: search to see if a particular package is installed and the version

-   rpm -ql \[package]: will query and list the files and folders associated with a given package (no sudo required)

-   rpm -qa --last: will show the packages that were last installed or upgraded

    -   rmp -qa: will show everything

-   rpm -evv \[package]: to remove individual package files.

-   rpm -qd \[package]: list documentation file locations for a package

    -   rpm -qdf \[/path/to/file]: list documentation containing a \[filename].

-   rpm -Va: verify rpm packages

-   rpm --import: to import keys

-   rpm --qa  gpg-pubkey*: to show the gpg-public keys that have been imported

-   If receive a message stating that the **rpm** db has been corrupted, do the following ...

    -   remove offending dbs
    -   `rpm --rebuilddb`: rebuild the rpm DB


### Shell Scripting

  ```sh
  while read HOST; do
    ping -c 3 $HOST
  done < myhosts
  ```


  ```sh
  for this in 1 2 3 4 5; do
    echo "This is line # $this"
  done
  ```


-   `export PATH=$PATH:/your/scripts`

## User & Group Management

### Create, Delete, and Modify Local User Accounts

-   useradd \[username]: add a user

    -   useradd -m -d /home/\[username] -s /bin/bash -c "Full Name of User" \[username]

-   adduser \[username]: works in newer OSs

    -   Gives more info about the user being created

    **NOTE**: Nolonger included in Arch Linux

-   userdel \[username]: will not delete the home dir of the user

-   userdel -r \[username]: This will remove the user and the user's home dir

-   Defaults for `useradd` can be found: `/etc/defaults/useradd`

-   chage -d 0 \[username]: force the user to change their password on next login


### Create, Delete, and Modify Local Groups

-   groupadd \[group-name]

-   Can edit the `/etc/group` file directly.

-   gpasswd \[group-name]: to edit the password for a group

-   newgrp \[group-name]: to set permissions to a new group.

    -   There will be a password

-   chgrp \[group-name\] \[filename\]: to change the group ownership for a file


### Use sudo to Access the root Account

-   visudo: This will check syntax to make sure that nothing has been done that will prevent system login.

-   sudo su -: From a logging perspective, this is the best way to become the root user.

-   Edit the `/etc/group` file directly and add the user to the *sudo* group.