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
