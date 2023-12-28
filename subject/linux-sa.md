# Linux Sysadmin

## Resize root partition
.. ref https://askubuntu.com/a/119458
.. extend hard-drive (ex on a VM drive)
.. use live OS boot, eg: install/rescue CD
fdisk /dev/sda
.. delete partition and next partitions if not at end
.. create the extend-partition
.. - note: must start the same block and not to remove existing signature
.. write new partition table
e2fsck -f /dev/sda1
resize2fs /dev/sda1

## Create swap
mkswap -L swap /dev/sda2
