## [WSL Backup](https://www.windowscentral.com/how-backup-windows-subsystem-linux-wsl-distribution)
```shell
$ wsl -l -v # see the installed distros

$ wsl --export Ubuntu(distro) Ubuntu20.04-2022-11-13.tar(filename)

$ wsl --import Ubuntu(distro) C:\Users\MyPC\AppData\Local\Packages\Ubuntu (install location) C:\Users\MyPC\Documents\ubuntubackup.tar (file location)
```
