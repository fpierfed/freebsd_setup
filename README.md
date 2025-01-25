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

Create users fpierfed and alicia and set their smb passwords
* smbpasswd -a fpierfed
* smbpasswd -a alicia

Create user-specific datasets
* zfs create TimeMachine/backup/fpierfed
    * chown -R fpierfed:fpierfed /mnt/TimeMachine/backup/fpierfed
* zfs create TimeMachine/backup/alicia
    * chown -R alicia:alicia /mnt/TimeMachine/backup/alicia


## Periodic
* Enable ZFS checks/scrubs
  * Copy periodic.conf to /etc/periodic.conf (this is the override to /etc/defaults/periodic.conf, by the way)



## Resources
* [Reddit post on TimeMachine setup archived](Screenshot_2025-01-20_at_08.25.46.png)
