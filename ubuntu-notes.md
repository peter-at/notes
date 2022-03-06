# notes on Ubuntu

# package info

[helpful info site](https://www.digitalocean.com/community/tutorials/ubuntu-and-debian-package-management-essentials)

## get info about packages

**pkg summary**
> `apt-cache show PACKAGE`

**pkg info w/deps**
> `apt-cache showpkg PACKAGE`

**content of pkg**
> `dpkg -L PACKAGE`

## query packages (dpkg-query)

**installed package and their priority**
> `dpkg-query -Wf '${Package;-40}${Priority}\n' | sort -b -k2,2 -k1,1`

**grep of optional and extra packages**
> `dpkg-query -Wf '${Package;-40}${Priority}\n' | sort -b -k2,2 -k1,1|awk '$2 ~ /optional|extra/ { print $1 }'`

truncated formatted list
> dpkg-query -Wf '${Package;-15}  ${Version;-10}  ${Priority}\n'

general query
```
$ dpkg-query -l |head |cut -b 1-100
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                                 Version                            Architecture Description
+++-====================================-==================================-============-===========
ii  accountsservice                      0.6.55-0ubuntu12~20.04.5           amd64        query and m
ii  adduser                              3.118ubuntu2                       all          add and rem
ii  alsa-topology-conf                   1.2.2-1                            all          ALSA topolo
```

## use-cases

**removing old kernel packages**
find installed kernel packages
> dpkg --list | egrep -i --color 'linux-image|linux-headers|linux-modules' | awk '{ print $2 }'
exclude ones that's not the installed kernel
> dpkg --list | egrep -i --color 'linux-image|linux-headers|linux-modules' | awk '{ print $2 }' |grep -v `uname -r`
remove kernel package not used/needed
> sudo apt purge linux-image-5.4.0-73-generic linux-modules-5.4.0-73-generic linux-modules-extra-5.4.0-73-generic





# remove packages

## no action check on removing packages

> `sudo apt-get --simulate purge PACKAGE`


# tasks

**show tasks**
> `tasksel --list`

**show commands to install a task**
> `sudo tasksel -t install server`

**show commands to remove a task**
> `sudo tasksel -t remove ubuntu-desktop`



