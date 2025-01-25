# FreeBSD 14.x New Server Setup

## General
* Install packages (see [packages.txt](packages.txt) for full list)
    * pkg install -y sudo zsh neovim python samba419 avahi-app nss_mdns rsync cmdwatch
* Configure packages
    * /usr/local/etc/sudoers
    * /boot/loader.conf
    * /etc/rc.conf
    * /etc/ssh/sshd_conf
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


## Periodic
* Enable ZFS checks/scrubs
  * Copy periodic.conf to /etc/periodic.conf (this is the override to /etc/defaults/periodic.conf, by the way)



## Resources
* [FreeBSD forum post on TimeMachine setup archived](Screenshot_2025-01-20_at_08.25.46.png)
* [Reddit post on avahi setup archived](Screenshot_2025-01-25_at_10.02.58.png)
