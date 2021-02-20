# distro.md

# deb based

## packages

* apt commands
```
# list installed
$ #apt list --installed
$ #apt list --installed pattern
```

* dpkg commands
[source 1](https://www.cyberciti.biz/howto/question/linux/dpkg-cheat-sheet.php)
```
# install or upgrade
$ #dpkg -i {.deb package}
# list
$ dpkg -l '*user*'
ii  adduser           3.118ubuntu2       all          add and remove users and groups
un  krb5-user         <none>             <none>       (no description available)
ii  xdg-user-dirs     0.17-2ubuntu1      amd64        tool to manage well known user directories
# list files in pkg
$ #dpkg -L {package}
# what package provides /path/to/file
$ #dpkg -S {/path/to/file}
```