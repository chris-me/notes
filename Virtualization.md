- [Cloning VM's](#cloning-vms)
  - [GNU/Linux](#clone--convert-virtual-disk)
- [VirtualBox](#virtualbox)
  - [Convert Virtual Disk](#clone--convert-virtual-disk)
  - [Shrink Virtual Disk](#shrink-virtual-disk)

<br>

# Cloning VM's

## GNU/Linux

- Clone / Copy the VM (make sure new MAC's are generated)
- Change the hostname (boot without network):
  - Ubuntu: adjust /etc/hosts and /etc/hostname --> reboot
  - RHEL / CentOS: use the nmtui command line tool
- Delete all SSH Keys (users and at global):

```
sudo rm -v /etc/ssh/ssh_host_*
```

- Recreate global SSH Keys:
  - Generally they should be recreated next boot when deleted
  - For Ubuntu one can run:

```
sudo dpkg-reconfigure openssh-server
```

# VirtualBox

## Clone / Convert Virtual Disk

```bash
VBoxManage clonehd --format vdi /path/to/original.vmdk /path/to/converted.vdi
```

- more at: http://www.rootz.de/2010/09/virtualbox-vmdk-images-zu-vdi-images-convertieren-einfach-schnell/

## Shrink Virtual Disk

### Ubuntu / Linux VM

- install zerofree
- boot vm in recovery modus
- run zerofree, for example with:

```bash
zerofree /dev/sda1
```

On the host:

```bash
VBoxManage modifymedium disk somehdd.vdi --compact
```

- more at: https://ubuntuforums.org/archive/index.php/t-908128.html
