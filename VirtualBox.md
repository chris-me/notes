## Shrink Virtual Disk

### Ubuntu / Linux VM

- install zerofree
- boot vm in recovery modus
- run zerofree, for example with:

```bash
zerofree /dev/sda1
```

- run "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" modifymedium disk somehdd.vdi --compact
- more at: https://ubuntuforums.org/archive/index.php/t-908128.html
