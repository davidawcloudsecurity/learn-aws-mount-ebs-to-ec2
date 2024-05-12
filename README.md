# learn-aws-mount-ebs-to-ec2
How to add storage to EC2 instance with EBS

Use the `lsblk` command to view your available disk devices and their mount points (if applicable) to help you determine the correct device name to use```ruby
**lsblk**
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
xvda    202:0    0   10G  0 disk
├─xvda1 202:1    0    1M  0 part
├─xvda2 202:2    0  200M  0 part /boot/efi
├─xvda3 202:3    0  600M  0 part /boot
└─xvda4 202:4    0  9.2G  0 part /
xvdb    202:16   0   10G  0 disk

or

**sudo lsblk -f**
NAME    FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
xvda
├─xvda1
├─xvda2 vfat   FAT16       7B77-95E7                             192.8M     3% /boot/efi
├─xvda3 xfs          boot  2e0d9ec9-a82a-43cc-a8e2-c6db30e7f6a4  375.1M    30% /boot
└─xvda4 xfs          root  497ad222-04fa-453f-b110-ba8001d38788    7.8G    15% /
xvdb

Use the file -s command to get information about a specific device
If the output shows simply data, as in the following example output, there is no file system on the device
**sudo file -s /dev/xvdb**
/dev/xvdb: data

```
## Resource
https://docs.aws.amazon.com/ebs/latest/userguide/ebs-using-volumes.html
