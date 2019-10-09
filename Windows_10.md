## Windows 10 Pro Version

Windows 10 Installer does not show Pro or other versions? May be b/c versions are "burned" into the BIOS of the corresponding PC.
To make them show up anyway:

Inside the 'sources' folder, create a file named *ei.cfg* w/ following content:

```
[Channel]
Retail
```

Sources:

https://www.askvg.com/fix-cant-select-windows-10-pro-edition-during-clean-installation/
