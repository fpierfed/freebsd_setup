# FreeBSD 14.x New Server Setup

## General
* Install packages (see [packages.txt](packages.txt) for full list)
    * pkg install -y sudo zsh neovim python samba419 avahi-app nss_mdns rsync cmdwatch
* Configure packages
    * /usr/local/etc/sudoers
    * /boot/loader.conf
    * /etc/rc.conf
    * /etc/ssh/sshd_conf
    * /usr/local/etc/smartd.conf
* (Re)start services
    * service avahi-daemon start
    * service dbus start
    * service sshd restart


## Timemachine
[Instructions](https://forums.FreeBSD.org/threads/samba-functions-but-unable-to-use-it-as-a-macos-time-machine-destination.79896/post-655905 (archived below))

* Copy timemachine.service to /usr/local/etc/avahi/services
* Copy smb4.conf to  /usr/local/etc/smb4.conf

Setup Time Machine destination pool + dataset
* zfs create TimeMachine/backup 

Create users user1 and user2 and set their smb passwords
* smbpasswd -a user1
* smbpasswd -a user2

Create user-specific datasets
* zfs create TimeMachine/backup/user1
    * chown -R user1:user1 /mnt/TimeMachine/backup/user1
* zfs create TimeMachine/backup/user2
    * chown -R user2:user2 /mnt/TimeMachine/backup/user2


## Other SMB Shares (e.g. media)

* Copy smb4.conf to /usr/local/etc/smb4.conf

Create users (if not created already), both local and smb and add them to the 
staff group:

* sudo pw usermod user1 -G staff[,other groups]
* sudo pw usermod user2 -G staff[,other groups]
* sudo smbpasswd -a user1
* sudo smbpasswd -a user2

Create the share directory / zfs dataset and give it the right permissions:
* sudo zfs create TimeMachine/media
* sudo chgrp -R staff /mnt/TimneMachine/media
* sudo chmod 2770 /mnt/TimeMachine/media

Note that the 2 in front of the 770 mask is equivalent to g+s, meaning that 
chmod 2770 === chmod g+rwxs u+rwx


## Periodic
* Enable ZFS checks/scrubs
  * Copy periodic.conf to /etc/periodic.conf (this is the override to /etc/defaults/periodic.conf, by the way)



## Resources
* [FreeBSD forum post on TimeMachine setup archived](Screenshot_2025-01-20_at_08.25.46.png)
* [Reddit post on avahi setup archived](Screenshot_2025-01-25_at_10.02.58.png)
* [Samba Wiki on simple share setup](https://wiki.samba.org/index.php/Setting_up_Samba_as_a_Standalone_Server)
* [UNIX permissions](https://www.redhat.com/en/blog/suid-sgid-sticky-bit)
