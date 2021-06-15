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

## query packages

**installed package and their priority**
> `dpkg-query -Wf '${Package;-40}${Priority}\n' | sort -b -k2,2 -k1,1`

**grep of optional and extra packages**
> `dpkg-query -Wf '${Package;-40}${Priority}\n' | sort -b -k2,2 -k1,1|awk '$2 ~ /optional|extra/ { print $1 }'`


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



