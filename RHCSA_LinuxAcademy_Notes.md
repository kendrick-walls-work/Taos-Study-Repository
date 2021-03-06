# Red Hat Certified Systems Administrator - RHCSA (EX200) Exam Prep

##   Essential Tools

###  Use Input/Output Redirection
     Redirection '>', '<' takes as args stdout.
     Redirection '2>', '2>>' takes as args stderror.
     Redirection '>> x' appends to file x if it exists, else creates said file and appends to it.
     Redirection '&>', '&>>' takes BOTH stdout and stderror as args.
     Redirection "|" means piping, which takes stdout as input.
     "head" and "tail" puts to stdout the first or last 10 messages in a given file (this can be adjusted with other flags)
     
###  Use grep and Regular Expressions to Analyze Text
     "grep -i x" means search for "x" ignoring case
     "grep -v x" means search for everything NOT "x"
     "grep ^x" means search for all lines starting with "x"
     "grep x$" means search for all lines that end with "x"
     "grep [ab]x" means search for everything with "ax" or "bx"
     "grep [^ab]" means search for everything BUT what is "ab"
     "grep 'a' | grep 'b' | grep 'c' | ..." means grep iteratively with filters of 'a', then 'b', then 'c' and so on.
     
###  Archive, Compress, Unpack and Uncompress Files Using tar, star, gzip and bzip2
     tar is for archiving purposes, but does not actually handle compression
     "gzip x" and "gunzip x" will compress / uncompress file or archive x
     You can compress individual files, but not directories (that's what archiving is for!)
     "tar -cvf archivename.tar /dirname/ /filename1/ /otherfilenames/" will tar the dirname directory and files filename1 / otherfilenames verbosely, and name the result archivename.tar
     the -z flag will pass the archive through gzip.
     the -j flag will pass the archive through bzip
     gzip is more universal for compression, though bzip2 is slightly better (albeit less reknown)
     "tar -xvf archivename.tar" will extract the contents of archivename.tar
     "tar -xzvf archivename.tar.gz" will run archivename.tar.gz through gzip, and then extract it
     star is tar, but for larger filesystems. Command "star -x" will not extract files that are older than what already exists in the directory 

###  Log in and Switch Users in Multiuser Targets
     When you first log into a system, you are in a login shell
     SSHing always results in an interactive shell.
     login shells (.bash_profile) is good for settings certain things up upon first login to the system
     interactive shells (.bashrc) is good for everything else.
     .bash_profile can be used for setting up environmental variables
     .bashrc can be used for setting up everything else.
     If you are SSHing into a machine for the first time, it will run both the ineractive AND login shell
     switching users from within the machine will not necessarily run the login shells upon changing

###  Create and Edit Text Files
     yum install "x" to do stuff.
     Why am I learning how to use nano?
     vim ftw
     At the very least, vi will always be available. ViM, maybe.
     :%s/word_to_replace/word_to_be_replace_with[/g for global, else only for first occurence in each line of file]

###  Create, Delete, Copy and Move Files and Directories
     yum install tree to use the tree command
     mkdir -p dir/sub_dir1/sub_dirA/..... to make directories with subdirectories
     mv command is how you rename files

###  Create Hard and Soft Links
     sym or symbolic links are like Windows Shortcuts
     ln command can be used to create links. 
     ln -s will create a soft link
     ln by itself will create a hard link by default
     when opening a sym link, the permissions of the source file are what the system atually reads when accessing
     so when you change a hard link, it'll change the SOURCE file
     hard links link directly to the inode source on the hard drive.
     if the source is removed, the hard link still exists and can be accessed until ALL references to said inode are gone
     if the source is removed, the sym link will be unable to locate the source inode, and accessing said 'source' will create and overwrite a new file with the same name 

###  List, Set and Change Standard UGO/RWX Permissions
     rwx|rwx|rwx has three sections: 1st is owner, 2nd is group, 3rd is other/everyone else
     whoamicommand can be used to figure out what your current username is
     chmod can be used to change permissions with octal (0-7) values
     chmod u/g/u +/- r/w/x 
     groupadd x to add a group named x
     to set user and group ownership of certain files/directories, use chown 
     chown ownername:groupname filename
     to add a user to a group, use usermod
     usermod -G groupname username (-G will change their secondary group)
     all sub-files and directories created in a directory will take on its parent's permissions upon time of creation
     for chmod numerical commands (like 777): read=4, write =2, execute=1
     giving owner rwx, group rx, and other r, it's chmod 754
     setgid allows for executing a file as the group who owns the file
     setuid allows for executing a file as the owner of the file, not the user (think a generic user running a file owned by root)
     chmod u+s/u-s filename for setuid
     chmod g+s/g-s filename for setgid
     The sticky bit prevents accidental deletion of things
     The sticky bit can be set with chmod +t filename
     for chmodding numerically, we'll have four digits. The first digit follows the same guidelines as for 3 digit notation: setuid/user=4,setgid/group=2,stickybit=1. The other digits refer to the 3 digit numerical permissions (read=4,write=2,execute=1)
     Hence, chmod 7777 means "set sticky bit, setuid, setgid, and all read/write/execute permissions for user/group/other 
     
###  List, Set and Change Standard UGO/RWX Permissions: umask
     umask = usermask
     umask deals with default permission settings
     0777 and 777 are considered the same thing
     setting permissions with umask only works for the current login session.
     umask follows the same general format as chmod, but the numbers are opposite. This means that umask 000 gives everyone rwx permissions upon creating a new file, umask 007 means everyone but other can do rwx, and umask 0022 means owner can do rwx, but group and other can only do rx
     files don't need execute permissions (hence the default of 666)
     by default, files have permissions of 666 ( no need to execute files), and directories have 777 (need execute permissions to traverse into them)
     umask simply indicates what permissions AREN'T allowed upon creating a file.
     chmod will look to your umask settings to determine what settings to follow
     root user has uid of 0
     root user also has a group name of "wheel". All other (non-admin) users have their username as a group name.
     To set a persistent (not just for the current session) umasks, change the values in .bashrc and .bash_profile

###  Locate, Read and Use System Documentation with man, info and /usr/share/doc
     apropros x to look for man page entries for x
     mandb to update the cache of man pages
     info command is basically an updated version of man
     type ? while in 'info' for help
     info --apropos=x will look for info pages with keyword x
     many programs' documentation is also available in /usr/share/doc
     locate command can be updated with updatedb
     to install updatedb, use yum install mlocate
     which command will show full paths
     whatis command will show man page descriptions
     locate command is pretty selfexplanatory
     whereis command will show binary, soure, and man page files for a command
     rpm -qd x will show information and documentation files for rpm-installed programs

###  Finding Files with Locate and Find
     locate can be installed via yum install mlocate
     locate's cache can be updated with updatedb
     find command is much more specific than locate command
     find /dir -name keywordname will find all files with kewordname in /dir
     find /dir -user keywordname will find all files with keywordname as user in /dir
     find [.,/] refers to current directory(.) or entire root directory(/)
     stat command can be used to show info about file
     find / -mtime -3/+3 means find in root directory all files that have either been modified in the LAST 3 days or are OLDER than 3 days
     man find is an INCREDIBLY useful resource
     find / -user jeff -type f -exec cat {} \; means FIND all files from user jeff, and execute the cat command of everything found
     find / -user jeff -type f -exec cp {} /home/mary \; means FIND all files from user jeff in root directory, and execute the copy command of everything found, and place it all into directory /home/mary 
     find / -user jeff -type f -exec rm {} \; will FIND everything of type file in the root directory from user jeff, and remove everything found

##   Operate Running Systems

###  Boot, Reboot and Shutdown a System
     systemd is the backbone of most modern linux distros
     reboot = reboots the computer.
     reboot = systemctl reboot
     shutdown -r/-p/ means reboot or power off (shutdown)
     shutdown -r +5 "Insert message here" means the computer will reboot in 5 minutes, and will send the message "Insert message here"
     shutdown -c will cancel any scheduled commands
     shutdown -h is basically the same thing as -p (halt and poweroff)
     shutdowns and reboots and whatnot are basically utilising targets (which was preceded by init levels)

###  Boot Systems into Different Targets Manually
     usr/lib/systemd/system contains many of the default targets
     multi-user.target - multiple users can be logged in on a system, with a text-based interface
    graphical.target - calls systems and services needed to launch a graphical interface
     emergency.target - boots into root cmd, mounts filesystem as read only
     rescue.target - basically safemode for Windows
     A target is just a grouping of dependencies for unit configuration files, services, sockets, and other targets.
     "systemctl isolate x" to switch to target x
     "systemctl get-default" to find default target
     "systemctl set-default x" to set default target to x
     To interrupt the boot process and specify another target: 
     1. reboot
     2. interrupt the boot process by pressing any key to stop the bootload counter
     3. Hit 'e' to edit the selected OS (the normal one)
     4. scroll down until you see line beginning with linux16 blah blah blah (this is the kernel)
     5. go to the end of that line, and type "systemd.unit=x" where x is the target you wish to boot into
     6.  Enter ctrl+x to continue the boot process
     7. You're in

###  Interrupt the Boot Process to Gain Access to a System
     To change the root password of a system (you'll need to physically be in front of a computer to do this):
     1. Enter the GRUB boot menu, and type 'e' to edit
     2. Go to the line that begins with 'linux16' (the kernel line)
     3. at the end of that line, type rd.break
     4.  hit ctrl+x
     4. hit ctrl+x to be booted into initramfs shell
     the contents of what will be our root is at this moment mounted as sysroot
     the sysroot is also mounted at this time as read-only
     5. remount with "mount -oremount,rw /sysroot"
     Now we'll need to mount our sysroot directory locally as my root directory, in order to make use of the /etc/passwd
     6. Now type "chroot /sysroot"
     7. Now you can type "passwd root" or "passwd"
     8. Retype your new password for root
     However, we're not done yet. When we changed our root password with passwd, it changed the context of SELinux, which RedHat and CentOS systems all use in order to tell Linux who has what permissions to view or execute certain items
     To overcome this, we'll need to relabel all our files. To do this, we need to make sure it exists.
     9. Enter "touch /.autorelabel"
     10. Type "exit"
     11. Type "exit" one last time to exit the initramfs shell
     12. Wait for the system to reboot with your newly changed root password. 

###  Identify CPU/Memory Intensive Processes, Adjust Process Priority and Kill Processes - Part 1
     ps is used to find and list processes running on a system
     "pgrep x" shows list of processes running that include the keyword x
     "pgrep x -l" shows list of processes (with name of process) running that include the keyword x
     "pgrep -u username -l" shows list of processes (with name of process) that user username has running
     "pgrep -v -u root -l" shows list of all processes (with name of process) that user root does NOT have running
     "pkill x" will grep for ALL processes with name "x" and kills them.
     "pkill" without any flags will default to "pkill -15", aka pkill "SIGTERM" (found via kill -l)
     SIGTERM will ask a process to die, but cleanly (finish first, then die)
     SIGKILL is for killing a process immediately, with no questions asked (useful for hung processes or malicious ones)
     SIGHUP will report termination of a process (like closing a terminal window)
     SIGINT (keyboard interrupt) is what is sent after pressing ^C on a terminal (ctrl+C)
     SIGQUIT will ask a process to quit.
     SIGCONTINUE and SIGSTOP will stop and start processes again
     SIGTSTP sends a stop that can be ignored (unlike SIGSTOP)
     "w" will show list of current users on the system
     you can kill other users' processes with these tools

###  Identify CPU/Memory Intensive Processes, Adjust Process Priority and Kill Processes - Part 2
     "jobs" can show what processes are running in the background of the current terminal window
     "kill -SIGSTOP %1" will stop the job running with id 1
     "kill -SIGCONT %1" will start up said job of id 1
     The NICE level shows the priority level of processes running on a system. The lower the NICE level, the higher the priority.
     use "man ps" to find out more

###  Identify CPU/Memory Intensive Processes, Adjust Process Priority and Kill Processes - Part 3
     NICE levels go from -20 to 19 (most favorable to least)
     As in the previous section, the lower the NICE level, the higher the priority for the system
     "nice -n num x" will set the NICE level of all processes of name x (and subsequent processes that x itself starts) to num
     "renice -n num processid" will change the NICE level of processid to num WITHOUT restarting the process
     To renice multiple processes - "renice -n num ${pgrep x}". This will effectively renice all the processes listed in the subshell (indicated by ${}) to what is specified as num
     Only priviledged users (su, root) can increase priority (decreasing NICE levels). Everyone else can only decrease priority (by increasing NICEness).

###  Identify CPU/Memory Intensive Processes, Adjust Process Priority and Kill Processes - Part 4
     "uptime" will also show uptime and system load averages. The load averages indicate the average system load for the past 1 minute, 5 minutes, and 15 minutes
     Since the load average shows TOTAL load average for the system, make sure to divide by the number of CPU processors the system has. This can be determined with "cat /proc/cpuinfo"
     A better way of determining # CPU processors is with "cat /proc/cpuinfo | grep "model name" | wc -l". This will concatenate to the screen CPU info, grep it for "model name"s (generally one per processor), pipe it to a wordcounter, and then list how many there are
     "top" will show the list of processes, along with their strain on the CPU (like Windows Task Manager!)

###  Locate and Interpret System Log Files and Journals
     /var/log is location of traditional log files
     man systemd-journald
     journald tool is run with "journalctl". When rebooting the system, journald gets removed - thus it is not persistent
     /run/log/journal/ is where journald stores its stuff
     because the /run/ filesystem is ephemeral, think of it like a Temp folder or where RAM is stored
     to make your journalctl persistent (i.e not wiped on reboot), go to /etc/systemd/ and change the line "#Storage=auto" to "Storage=persistent"
     This will change the location of stored logs to /var/log/journal
     "journalctl -n" shows last 10 messages
     "journalctl -xn" shows last 10 messages, plus extra explanations if available
     "journalctl -f" shows last 10 messages, while continuously listening
     "journalctl _SYSTEM_UNIT=x" will allow you to listen to ANY Unit Configuration Type x
     To find the available system unit types, type "systemctl -t help"
     "/etc/rsyslog.conf" is what determines what type of log goes to where.
     to further query those types of logs, we can utilise "journalctl -p x" (where x is the type of log specified in rsyslog, like info)
     "journalctl --since=yesterday" will show all logs...since yesterday. NOTE: If journalctl is not persistent, you're not gonna get accurate results (unless the system was on since at least yesterday)
     "systemd-analyze" will show how long it took to boot up
     "systemd-analyze blame" will show how many configuration files went through the boot process, along with how long each took 

###  Access a Virtual Machine's Console
     Use Virtual Machine Manager if it's installed. If not...then this video was pretty much useless lol

###  Start and Stop Virtual Machines
     When working with KVM and the Virsh manager, you'll need to log in as root
     type "virsh" to start
     You'll be able to view all the VMs owned by a given user (if you're root, you can see everyone's)
     after typing "virsh", type "help" for help, and "list --all" will show ALL (running AND stopped) VMs
     to shutdown a VM, enter "shutdown x" where x is the VM's name
     to start a VM, enter "start x" where x is the VM's name
     ...You can also do it via the GUI version 

###  Start, Stop and Check the Status of Network Services
     "systemctl list-units | grep network" to show list of network related services and targets
     In general, it is best practice to ensure that multiple network related services are running on different servers (so if one goes down, you're not completely screwed)
     You can restart the network with "systemctl restart network.service". If you don't include the .service at the end, the default will assume it IS .service
     to check if it is enabled, simply run "systemctl is-enabled network"
     to ensure a service is able to run at bootup, type "systemctl enable x"
     You can double check this with "systemctl is-enabled x"
     "systemctl status x" will show the status of x
     Multiple types of services / programs make use of the network (CIFS, HTTPD, VSFTP) 

###  Securely Transfer Files Between Systems
     Both "scp" and "sftp" go over port 22, which is the SSH port (aka it's secure)
     "scp filename username@user_ip_address:/dir/dir1/" will securely copy filename over to user username at user_ip_address in directory /dir/dir1
     "sftp user@user_ip_address" will connect you to user_ip_address as user
     to DOWNLOAD a file, type "get x"
     to UPLOAD a file x, type "put x"

##   Configure Local Storage

###  List, Create and Delete Partitions on MBR and GPT Disks
     MBR-based partitions can only have 4 primary partitions. Each partitions is only 2 tebibytes in size
     Then came GPT-based. Whereas MBR was 32-bit, GPT was 64-bit.
     MBR = x32, 4 partitions, 2 Tebibyte limit per partition (slightly more than 2 TBs)
     GPT = x64, 128 partitions, 8 Zebibyte limit per partition (it's a really freaking big number of TBs)
     /dev shows all attached device storage
     fdisk can be used to manage MBR based partitions
     The beginning of a hard drive is usually reserved for MBR records in order to maintain records about the disk
     The end of a hard drive is for actual space / storage size
     "mkfs -t xfs /dev/partitionname" to format partitionname to xfs (the filesystem that RedHat prefers)
     "df -h" will show all the mounted devices
     "blkid" will show available block storage devices attached to your system
     generally place mountpoints in the /mnt filesystem
     "mount /dev/partitionname /mnt/mountlocationname" will mount partitionname to mountlocationname
     "umount /mnt/mountlocationname" to unmount the device mounted at mountlocationname
     Remember you can't be INSIDE the mount point if you wanna unmount it
     After doing ANYTHING with a partition (mount, resize, etc), best practice is to run "partprobe", which will reload partition info
     It is actually BETTER and SAFER to mount with "mount" by the UUID: "mount -u uuidgoeshere /mnt/mountlocationname" will only mount that specific partition, and not a lookalike
     It is actually BETTER and SAFER to mount with "mount" by the UUID: "mount -u uuidgoeshere /mnt/mountlocationname" will only mount that specific partition, and not a lookalike
     gdisk can be used to manage GPT based partitions

###  Create and Remove Physical Volumes, Assign Physical Volumes to Volume Groups  and Create and Delete Logical Volumes
     LVM = Logical Volume Manager
     A physical volume is a partition, device, or the entire disk. In order to use a physical volume, it must be initialized for the LVM to be able to use it. To help identify metadata about that physical volume, a label is placed in the second 512 byte sector of the volume. You can either have 0, 1, or 2 copies of this metadata, stored on each volume. By default, there is 1 copy. The first copy will be stored in the beginning of the device, the 2nd in the end (to prevent accidental overwriting) 
     A volume group is a combination of physical volumes that createas a pool of space that the LVM or logical volumes can allocate. It's made up of extents - the smallest unit of space that can be allocatable
     "pvcreate /dev/volumename" will create a physical volume from the given location
     "vgcreate groupname /dev/vol1 /dev/vol2" to create a volume group named groupname, from the various vol#s indicated
     "lvcreate -n volumename -L num[M/G/T] volumegroup" to create a logical volume of name volumename, size num (in Mega/Giga/Terabyte form), and from volume group groupname
     "pvdisplay" will show all available physical volumes
     "vgdisplay" will show all available volume groups
     "lvdisplay" will show all available logical volumes
     xfs filesystems cannot be decreased, ONLY increased
     ext filesystems can be both decreased AND increased 
     "lvremove /dev/volumepath" to remove the logical volume at volumepath
     "vgremove groupname" to remove the volume group by groupname
     "pvremove /dev/volumename" to remove the physical volume by volumename 

###  Configure Systems to Mount File Systems at Boot by UUID or Label
     "xfs_admin -L filesystemname /dev/filesystem" to create a label on an xfs filesystem
     "xfs_admin -l /dev/filesystem" will show the label of filesystem if it exists
     "tune2fs -L filesystemname /dev/filesystem" to create a label on an ext filesystem
     similar to xfs_admin,  use "tune2fs -l /dev/filesystem" to find label 
     "man fstab" to view mount options
     fstab can be found in /etc/fstab" 

###  Add New Partitions and Logical Volumes and Swap to a System Non-Destructively
     swap memory is the area of disk that can be allocated to be used by the Linux Kernel Memory System. If we have programs that are using more than the available amount of physical memory, the kernel can associate older processes / memory to the appropriate disk space. 
     It's generally good practice to allocate 2-3 times the amount of free memory as swap space
     To prepare a logical volume to be a swap space, we need to give it a swap signature
     to do this, "mkswap /dev/groupname/swappartition"
     and to turn it on/off, "swapon/swapoff /dev/groupname/swappartition"
     
##   Create and Configure File Systems

###  Create, Mount, Unmount and Use VFAT, EXT4 and XFS File Systems
     mkfs.vfat to format a partition with a fat filesystem
     Remember that a common question to ask oneself during the exam is "what happens if I reboot the machine? Will my settings be saved?"
     "mount /dev/partition1 /mnt/mountpoint" to mount partition1 to mountpoint
     don't foget to utilise /etc/fstab after mounting your partition in order to make it persistent (saved after a reboot)

###  Mount and Unmount CIFS and NFS Network File Systems
     Be sure to mount persistently
     CIFS mounts have two forward slashes and no colons
     NFS mounts have colons and don't have two forward slashes

###  Extend Existing Logical Volumes
     pvmove will map physical extents from one device to another from within the SAME volume group
     vgreduce to remove physical volumes from the volume group
     xfs_growfs will look for available disk space

###  Create and Configure Set-GID Directories for Collaboration
     directories with set-gid will be accessed by a user with group owner permissions, rather than the accessor themself
     chmod g+r/w/x groupname
     chown :newgroupowner directoryname

###  Create and Manage Access Control Lists (ACLs)
     By default, xfs and ext4 filesystems support ACLs (Access Control Lists)
     "getfacl file" to view permission stuffs about it
     "setfacl -m u:username:r/w/x/g:groupname:r/w/x filename"
     chmod will change the permissions of the mask
     "setfacl -m m: :r filename" to change the mask permissions for filename
     ACLs are useful if you have a user that does not belong to the file or directory's group, but still needs access to that specific thing
     A default ACL can be set on a directory. The default ACL will allow subdirectories and files of the parent to inherit the default permissions granted by the default ACL. 
     "setfactl -x u:username/g:groupname/o: filename" to remove the ACL associated to either the user, group, or other of a particular file

###  Diagnose and Correct File Permission Problems
     If we cannot delete a file: does it have the sticky bit activated? Are we running as a local user, or root?
     Whenever we update the ACL on a file or directory, the MASK changes (we can set this with setfacl). When MASK changes, permissions for users/groups/other may change 
     When you add a group ACL onto a file or directory, when you modify regular group permissions, it doesn't apply to the ACL entry of the groups. So when you modify the group ownership with chmod or setfacl, it's not going to change the subgroups associated with it if there are entries in those ACLs
     "chmod -R g+rwX /tmp" will set read write permissions to /tmp recursively, but will only allow execute permissions for directories in the tree of /tmp, not the files (this is a good security practice).
     the cp command does NOT preserve ACL rules
     the mv command DOES preserve ACL rules
     default ACL permissions are for inheritance - meaning that all subdirectories and files will keep the permissions of the parent directory

##   Deploy, Configure and Maintain Systems

###  Configure Networking and Hostname Resolution Statically or Dynamically: Troubleshooting
     ifconfig is being deprecated in Red Hat Linux 7
     "ip addr" is what will now be used instead of "ifconfig"
     "ip addr show x" for showing information about a specific port, like eth0
     Most commands here are for ipv4
     we can ping with ipv6 with "ping6"
     "traceroute x" can be used to investigate hops for a specific address. To install, use "yum install traceroute"
     alternatively, you can use "tracepath", but many routers do not support it
     "ss -a" will show all listening and established connections
     "ss -at" will show all listening and established TCP sockets
     "ss -au" will show all listening and established UDP sockets
     The "Peer Address:Port" will only show internal IP addresses, not external
     For "ss" commands, the "Peer Address:Port" will only show internal IP addresses, not external
     "ip -s" will show statistical information for a particular port used (i.e "ip -s link show eth0")

###  Configure Networking and Hostname Resolution Statically or Dynamically: Network Manager
     "nmcli dev status" will show the network command line interface for network devices
     double tab while typing any command to get the closest matches
     "nmcli con add con-name "mycon" autoconnect yes type ethernet ifname eth1" To create a new connection named "mycon", of type ethernet, and connection port name of eth1
     "cd /etc/sysconfig/network-scripts/" to obtain configuration information for various connections in system
     By default, the script listed in /etc/sysconfig/network-scripts will be booted with a "BOOTPROTO" of dhcp
     to set the ip address and gateway, include the tags "ip4 ip.address.here gw4 gateway.address.here"
     "nmcli con up/down conname" to bring up or put down connections of name conname
     "nmcli con delete conname" will delete the connection conname

###  Configure Networking and Hostname Resolution Statically or Dynamically: Hostname Configuration
      DNS poisoning is where you edit the /etc/hosts file by having the system connect to a specific ip for a given dns (only if the system is configured to connect to the hosts file before looking at the nameserver) 
     If our connection uses DHCP, then we'll need to modify both the /etc/resolv.conf and the /etc/nsswitch.conf
     "hostnamectl set-hostname domainname" will change our hostname
     "hostnamectl status" will show the status of the host
     nameservers allow us to look at external IP addresses

###  Schedule Tasks Using at and cron
     "yum install at" to install at. At is a task scheduling program
     at is a daemon, so make sure to enable it, and set it to run on bootup
     if a user's name is within at.deny or cron.deny, they are not allowed to make use of either service. If the at or cron.deny does not exist, then only the users available in at.allow or cron.allow are allowed to use said service (usually root is allowed by default) 
     crontab allows a user to schedule their own cron jobs.
     systemcron is available via /etc/crontab
     cron jobs can only run while the computer is on
     "/etc/crontab" shows a template as to how to run a cron job
     a user cron job does not have the user-name field
     * * * * * refers to every minute/hour/day of month/month/day of week
     */5 * * * * refers to every five minutes of every hour/day of month/month/day of week
     5 0 */2 * * will run every two days, at 12:05am of every month
     (i.e 1st, 3rd, fifth, seventh, etc)
     5 0 4-12 * * will run every 4th-12th of the month at 12:05AM
     5 0 * * fri will run every friday at 12:05AM
     All scripts in cron.daily/hourly/monthly/weekly will run every...daily/hourly/monthly/weekly of every day.
     cron.d will contain any custom cron jobs that do not fit the aforementioned templates
     "rpm -qc x" to find the location of configuration files for x
     anacron is only available for priviledged users. It is responsible for scheduling the daily/weekly/monthly cron jobs. Anacron will schedule to run a job after a specific amount of time after last execution

###  Start and Stop Services and Configure Services to Start Automatically at Boot
     Make sure a service is enabled via "systemctl enable x"
     upon rebooting, said service x will be available during startup
     use "systemctl list-dependencies x | grep y" to look for dependencies of x, as well as whether or not y is among the list

###  Configure Systems to Boot into a Specific Target Automatically
     "systemctl get-default" will get the current default target (usually graphical.target if on a GUI, and multi-user.target for terminal based logins like SSH)
     "systemctl set-default x" will set the default target on startup to be x.

###  Install Red Hat Enterprise Linux Automatically Using Kickstart
     Every RedHat system comes with an anaconda-ks.cfg
     The settings in these anaconda-ks.cfgs are the configuration settings (like root, usernames, network stuff, harddrive stuff, bootloaders, partition stuff, packages to install, etc)
     "yum install system-config-kickstart" to create a GUI-based kickstart
     to access it, go to system-config-kickstart
     documentation for kickstart (aka pykickstart), type "rpm -qd pykickstart"

###  Configure a Physical Machine to Host Virtual Guests
     qemu is what actually makes virtualization occur
     "yum install virt-manager qemu-kvm qemu-img"
     "yum install libvirt libvirt-python python-virtinst libvirt-client" libvert is what allows us to interact with qemu and kvm. python-virt allows us to start required packages from within the VM
     "systemctl enable libvirtd" AND "systemctl start libvirtd"
     the aforementioned two commands are REQUIRED in order to boot a VM

###  Install Red Hat Enterprise Linux Systems as Virtual Guests
     Is the Virtual Machine Manager gonna be a GUI during the exam?
     If this works, I can say "screw you" to virtualbox
     if networking doesn't work for whatever reason (it wasn't set up, etc)
     use nmcli to find available networking connections, and modify the script as necessary in /etc/sysconfig/network-scripts/
     "nmcli con mod 'x' connection.autoconnect yes" will modify the script of 'x' to connect upon startup

###  Configure Systems to Launch Virtual Machines at Boot
     in order for KVM and all VMs to work, ensure that service "libvirtd" is enabled
     In order to enable it, do "systemctl enable libvirtd"
     type virsh to get into the virtualization interactive terminal
     from there, "list --all" to list all available VMs (both active and inactive)
     then, "autostart x" to automatically start x upon physical machine reboot

###  Configure Network Services to Start Automatically at Boot
     "systemctl status network" to check network status
     "systemctl list-units | grep network.target" to find the networking target
     systemctl list-dependencies multi-user.target | grep network" will list services that involve the network,
     "cd /etc/sysconfig/network-scripts" and "nmcli con show" to indicate which network services are currently available on the device
     cat the shown network service to check if the "ONBOOT" flag is set to yes or no.
     If yes, you're gucci. If no, set it to yet by typing "nmcli con mod 'x' connection.autoconnect yes"
     upon reboot, it ought to be connected on start
     if the question is referring to some other network related service, simply do the good ol' "systemctl enable x" to enable x

###  Configure a System to Use Time Services
     "timedatectl" will show information about the time and date
     "timedatectl list-timezones" will show all available timezones that the system can configure
     "tzselect" will help you figure out which timezone you are in (in case you don't know)
     "timedatectl set-timezone x" to set the timezone
     "timedatectl set-time hour:minute:second" to set the time to hour:minute:second
     "date" will show you the current date.
     "systemctl status chronyd" to see if chronyd is running or not
     chronyd works with the NTP time-server protocol.
     chronyc sources -v will show the actual servers that the program communicates with to figure out the time 
     "systemctl restart chronyd" to restart the service
     "chronyc sources -v" will update the servers accordingly
     The stratum is the number of hops a server needs to go through in order to reach the source server
     A stratum level of "0" is the actual source, like a GPS thingy. "1" is our first NTP server that communicates with the source of time. "2" is a system or machine that is connected to the NTP server, which in turn is connected to that ultra reliable source of time
     "chronyc tracking" will show more detailed information about the syncing process that "chronyc -v" would show
     In general, the lower the "stratum" level listed, the better.
     "Frequency" show how off the time really would be if NTP was not functioning
     NTP itself will either increase or decrease the counting time in order to account for any inaccuracies between real time and what is listed on the system itself
     To change the timeservers that chronyd uses, go to /etc/chrony.conf
     finally, to apply those changes, type "systemctl restart chronyd"

###  Install and Update Software Packages from Red Hat Network, a Remote Repository or the Local File System: YUM
     "yum check-update" will check for available updates
     "yum update -y" will automatically update everything that needs to be updated.
     However, blindly updating everything may break some stuff, especially in a corporate / professional environment
     Thus, checking for specific things to update is a smarter, more careful means of updating stuff
     "yum search x" will only search package details or descriptions that involve the term x
     If this fails, go to "yum search all x", which will search by descriptions, URLs, package names, details, and summaries for a specific term x
     "yum info x" will show specific information about the package x, including name, architecture, version, release, size, repo, etc
     For the exam, we'll be searching for red hat authorized repos and packages - this is part of why we have to pay for RH subscriptions in the first place
     "yum list installed | grep x" will look for all currently installed packages of pattern x (I say pattern because we grep'ped for it)
     "yum provides/whatprovides /dir/path/here" will look for any and all packages that would create a directory path of /dir/path/here
     to clean out cache info, "yum clean all"

###  Install and Update Software Packages from Red Hat Network, a Remote Repository or the Local File System: RPM
     The RPM package manager is used to build, install, query, verify, update, and erase individual software packages.
     To download the rpm package associated with a "yum search x" query, you can use "yumdownloader x"
     Havind downloaded the package name, you can install the package with "rpm -ivh x-package-name"
     -i stands for update, -v for verbose, and -h for the little shiny progress bar
     -U will update and upgrade if installed, -q for querying, -a will list "all"
     -qd will query for documentation
     "rpm -e x" will remove package x
     "yum localinstall x" will install x with the yum package manager (helpful with finding and downloading dependencies automatically)
     YUM is basically a package manager meant to complement RPM

###  Install and Update Software Packages from Red Hat Network, a Remote Repository or the Local File System: Managing Repositories
     /etc/yum.repos.d/ is where repository configuration files are kept for YUM
     "yum repolist" will show the list of currently enabled repositories on the system
     "yum repolist all" will show the list of ALL repositories on the system
     To disable a repo, make sure to type "yum-config-manager --disable repoid", which will disable the given repository ID. Another way is to simply change the configuration file for that specific repo by altering the "enabled" value from "1" to "0".
     Alternatively, to enable it, it's basically the same command, but with --enable instead,  and placing the "enabled" value of "1" instead of "0"

###  Install and Update Software Packages from Red Hat Network, a Remote Repository or the Local File System: Configuring a Local Repository
     To mount a read-only DVD ISO to be a block device, type "mount -o loop isoname.iso /mount/point to mount isoname.iso to the specified mount point directory
     Next, go to /etc/yum.repos.d, and create your own repo file
     Format is [name-of-repo] in line 1
     name=Type Repo Name Here in line 2
     baseurl=file:///mount/point in line 3
     enabled=1 in line 4
     gpgcheck=0 in line 5
     Now you can "yum install x" to utilise the local repositories listed in your CD.

###  Install and Update Software Packages from Red Hat Network, a Remote Repository or the Local File System: Configuring a Local Repository: Configure the GPG Key
     A GPG Key allows us to take a public, verified key and sign it against / verify the signature of the GPG key located in the repository for the package you're trying to install. 
     "yum-config-manager --add-repo http://repo-url-goes-here/" This will add a new repo, without a public GPG key
     Now cd to "/etc/pki/rpm-gpg", and "wget x" to add your public GPG key X / URL for GPG key x
     Now, having taken the full path of this new GPG key, go to /etc/yum.repos.d/, and to the repository that you just added.
     Open up that repository configuration file, and add "gpgcheck=1", and 'gpgkey="file://full/path/for/GPG/key/here"' in separate lines following the "enabled=1" line.

###  Update the Kernel Package Appropriately to Ensure a Bootable System
     To manually update it manually though, use "yum clean all" and "yum list kernel"
     "yumdownloader kernel"
     "rpm -ivh kernel-package-name.rpm"
     "dracut" will create another image of the kernel in /boot/ if it does not exist already
     "uname -r" will show the current kernel version.
     In most real life scenarios, you won't be updating your kernel manually, rather it's much easier just to do "yum upgrade"

###  Modifying the System Bootloader
     "yum list kernel" will show all available kernels in the system
     "grub2-set-default x" will set the default of the kernel to whichever was specified
     generally speaking though, a value of 0 for x will be the latest version of the kernel that is loaded by default by the GRUB bootloader
     "uname -r" will help you verify that this was indeed applied.

##   Manage users and groups

###  Create, Delete, and Modify Local User Accounts
     "id" will list id info about your current user
     root user will always have an id of 0
     users with an id of 1-200 are system users, for specific red hat processes / red hat file owned processes
     users with an id of 201-999 are for those who don't own files on the system but use system processes 
     /etc/passwd file lists all of the user accounts on the system, and information associated with that user account
     ex: "apache:x:48:48:Apache: /usr/share/httpd: /sbin/nologin".  "apache" is the user account name. "x" is the password field, but is linked to another file. "48:48" are the user and primary group ids. "Apache" is an arbritary field, but most of the time it is the first or last name of the user. "/usr/share/httpd" is the home directory of the user. "/sbin/nologin" is the user's shell.  In this particular case, /sbin/nologin means deny a user access to the shell.  
     Each user can only have one primary group. Whenver said user creates a new file or directory, the group owner will be that of the primary group that the user is part of.
     /etc/shadow will show a list of all user passwords. For actual users of the system, the password will be hashed.
     "useradd -" will create a new user, with various options and configurations. By default, any newly created user will have the /etc/skel copied over to it
     /etc/default/useradd will show the default configurations for creating a new user
     "passwd user" will change the password of user user
     "usermod --help" will show available options for the usermod command
     "userdel x" to delete user x

###  Change Passwords and Adjust Password Aging for Local User Accounts
     /etc/login.defs shows specific parameters for logins
     Three main tools for password policy management are passwd, usermod, and chage
     "usermod -s /sbin/nologin x" will disable shell access for user x
     "chage -l username" will show info about username
     "chage -E 2018-09-01 username" will set the account to expire on 9/1/2018.
     "chage -E -1 username" will remove the account expiration from username
     "chage -M num username" will set username's password to expire after num days
     "chage -d 0 username" will automatically expire username's passwords and force password changes
     "chage -I num username" will expire the password for username after num days

###  Create, Delete, and Modify Local Groups and Group Memberships
     "groupadd x" will create a new group named x
     "usermod -g groupname username" will change the primary group of username to groupname
     relog to apply changes
     "newgrp groupname" allows a user to change primary groups - A primary group is the group that the user is logged in as
     "groupdel groupname" to delete groupname. Keep in mind that you cannot delete a group that a user is currently using as a primary group. This is why by default, newly created users have their own name listed as a primary group
 
###  Using set-GID On Directories 
     "chmod g+s parentgroup/" will change the group owner of the current directory to that of parentgroup. This way, instead of having each individual user set their own parent group everytime (to allow other users from within the same group to access said files or directories), we can do it all at once for all future users.

###  Configure a System to Use an Existing Authentication Service for User and Group Information: Using Realmd
     "yum install -y realmd" in order to discover our active directory LDAP realm
     "yum update" or "yum upgrade" to update our packages
     "realm discover x" to check out information about our given active directory address x
     "realm join x" to join the active directory address x
     /etc/ssh/ssh_config has settings for AD stuff, such as useful kerberos options
     "systemctl restart sshd" to apply changes
     To test single sign on, ssh with the format listed in login-formats (which is shown when you use the 'realm discover x' command)
     authconfig-gtk is an alternative, GUI-based configuration program

##   Manage security

###  Configure Firewall Settings Using firewall-config, firewall-cmd, or iptables
     The netfilter module is what does the actual magic
     "yum install firewalld firewall-config"
     "systemctl start firewalld"
     "systemctl enable firewalld"
     If we make a run-level change on the firewall, it will not survive a reboot
     "firewall-cmd --get-zones" will show the "zones" of specific IP addresses
     "firewall-cmd --get-default-zone" to get default zone
     "firewall-cmd --list-all" will only show info with regards to your current default zone
     "firewall-cmd --zone=home --add-source=192.168.1.0/24" will add the IP address range listed in source, to the home zone
     this will only be a run-level change, however, unless you add the "--permanent tag"
     To make sure the --permanent change would stick, reload the firewall-cmd
     But to skip that, you'll have to do the same command twice - once for the runtime, and once for the permanent
     "firewall-cmd --zone=public --add-port=80/tcp --permanent" will add port 80/tcp to the public zone, and apply said settings permanently
     firewall-config is a GUI based version of firewalld (this only works for GUI based interface)
     "firewall-cmd --panic-on" to turn on panic mode. This will lock out EVERYTHING but the console.

###  Configure Key-Based Authentication for SSH
     The public key is used to verify our private key
     "ssh-keygen -t dsa/rsa" will generate a key with either dsa/rsa encryption. By default, rsa is used. 
     The importance of a passcode is to ensure that the private key is protected, as someone gaining unauthorized access to it will be able to compromise any services or systems that utilise the public key.
     "id_rsa.pub" is our public key. "id_rsa" is our private key. Keep the private key SAFE.
     "ssh-copy-id username@ip-address" to copy the public key to username@ip-address
     The cool thing here is that you can add commands to the end of ssh connections (like 'ls')
     "ssh-agent bash" and "ssh-add" will allow you to issue commands to your remote, SSH'd server without prompting for repeated key passphrases
     This will store the passphrase for the current login session
     Permissions on a private key need to be 600, Permissions on a private key need to be 644.

###  Introduction to SELinux
     SELinux has three modes: Enabled, Passive, and Disabled.
     Enabled means that SELinux is monitoring for policy infringements, or when a process is trying to access a resource on the system for which it doesn't have the proper access. SELinux both monitors and enforces these policies. This applies to booleans as well.
     Passive means that SELinux is monitoring and reporting these infringements if the process is accessing a resource outside of its assigned context. It will report, but not enforce. 
     Disabled means that SELinux isn't really doing anything. It must be configured via /etc/selinux/config, and requires a reboot. It is not suggested, but can be used to configure whether or not SELinux is the cause for a policy infringement
     By default, SELinux will be in Enabled mode.
     A boolean is: "a conditional rule that allows runtime modification of the security policy without having to load a new policy. For example, to allow cgi scripts to be executed then you can enable the httpd_enable_cgi boolean. The opposite is true if the administrator wants to just disable all cgi scripts on a system"
     Useful man pages are Booleans(8), Selinux(8), and Getsebool(8) 

###  Set enforcing and permissive modes for SELinux
     "getenforce" will show the current SELinux mode
     "setenforce 0" will change SELinux to Permissive mode (aka Passive). This will stay until the machine is rebooted/restarted. 
     "setenforce 1" will change SELinux to Enforcing mode (aka Enabled)
     /etc/selinux/config will show the default configuration for SELinux
     To disable SELinux, change the "SELINUX=" value in the config to "disabled", and reboot to apply.
     For the exam, you'll likely only really need getenforce and setenforce 

###  List and identify SELinux file and process context
     "ls -Z" will show SELinux context on the current directory
     "semanage fcontext -l" will list all the available contexts on the system, along with the files they are associated with.
     "(/.*)?" means it will assume the context of its parent directory. This is an optional configuration
     "restorecon x" will restore the context of x to that of its current directory
     "touch /.autorelabel" will cause everything in the filesystem to be relabeled upon reboot / restart

###  Restore Default File Contexts
     "semanage fcontext -a -t context_name_t '/directory/name(/.*)?' to add a context of type context_name_t to the /directory/name and everything underneath that particular directory.
     "restore -Rv /directoryname" will verbosely restore the contexts recursively within the directory
     This will make your new context persistent throughout future reboots
     "semanage fcontext -d "directory/name(/.*)?" to delete the context rule for the specified /directory/name/ path
     Finalize this with "restorecon -Rv /directory/name"

###  Use Boolean Settings to Modify System SELinux Settings
     SELinux Booleans can be used to enable/disable specific feature sets for a specific service or feature.
     "getsebool -a" to list all available booleans on the system
     "semanage boolean -l" will show the list of booleans, including their state, default state, and description
     "sed -i 's/disabled/public_html/' userdir.conf" will replace all instances of 'disabled' with 'public_html' in the 'userdir.conf' file
     "setsebool x on/off" will set the boolean value x on or off
     "setsebool -P x on/off" will set the boolean value x on/off, but persistently. 

###  Diagnose and Address Routine SELinux Policy Violations
     "yum install setroubleshoot-server" will  help to troubleshoot the loveliness known as SELinux
     This command will also enable a command known as sealert, the User Interface'd version of setroubleshoot
     /var/log/audit/ has an audit.log that will show information about SELinux. Here is where sealert comes in handy
     "sealert -a /var/log/audit/audit.log" will parse through the audit.log file that setroubleshoot-server populated and give user-friendly advice / info
     Basically, if you're doing stuff and have a general idea that SELinux is behind it not working (like those lovely seg faults in C), you now have your own personal valgrind-like audit.log, which is updated frequently thanks to setroubleshoot-server. From here, sealert -a /var/log/audit/audit.log will help you with general advice as to what is wrong, and what you might be able to do to resolve the issue.
