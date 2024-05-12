# learn-aws-mount-ebs-to-ec2
How to add storage to EC2 instance with EBS

Use the `lsblk` command to view your available disk devices and their mount points (if applicable) to help you determine the correct device name to use.
```ruby
lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
xvda    202:0    0   10G  0 disk
├─xvda1 202:1    0    1M  0 part
├─xvda2 202:2    0  200M  0 part /boot/efi
├─xvda3 202:3    0  600M  0 part /boot
└─xvda4 202:4    0  9.2G  0 part /
xvdb    202:16   0   10G  0 disk
```
or
```ruby
sudo lsblk -f
NAME    FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
xvda
├─xvda1
├─xvda2 vfat   FAT16       7B77-95E7                             192.8M     3% /boot/efi
├─xvda3 xfs          boot  2e0d9ec9-a82a-43cc-a8e2-c6db30e7f6a4  375.1M    30% /boot
└─xvda4 xfs          root  497ad222-04fa-453f-b110-ba8001d38788    7.8G    15% /
xvdb
```
Use the file -s command to get information about a specific device

If the output shows simply data, as in the following example output, there is no file system on the device
```ruby
sudo file -s /dev/xvdb
/dev/xvdb: data
```
Do not use this command if you're mounting a volume that already has data on it (for example, a volume that was created from a snapshot). Otherwise, you'll format the volume and delete the existing data.
```ruby
sudo mkfs -t xfs /dev/xvdb
```
If you need to preserve the data in /tmp
```ruby
# Create a temporary mount point
sudo mkdir /tmp_mount

# Mount /dev/xvdb to the temporary mount point
sudo mount /dev/xvdb /tmp_mount

# Move data from /tmp to the temporary mount point
sudo cp -p /tmp/* /tmp_mount

# Unmount the temporary mount point
sudo umount /tmp_mount

# Mount /dev/xvdb to /tmp
sudo mount /dev/xvdb /tmp
```

```ruby
sestatus; ls -Z
sudo systemctl status systemd-journald; sudo systemctl status systemd-journald.socket; sudo systemctl status systemd-journald-dev-log.socket; sudo systemctl status rsyslog
sudo systemctl stop systemd-journald; sudo systemctl stop systemd-journald.socket; sudo systemctl stop systemd-journald-dev-log.socket; sudo systemctl stop rsyslog
sudo systemctl status systemd-journald; sudo systemctl status systemd-journald.socket; sudo systemctl status systemd-journald-dev-log.socket; sudo systemctl status rsyslog
```
After mount /var
```ruby
sudo mkdir /var_mount; sudo mount /dev/$xvdb /var_mount; sudo cp -p /var/* /var_mount; sudo umount /var_mount; sudo mount /dev/$xvdb /var; sudo mkdir /var/log/journal
sudo journalctl --flush
journalctl --verify
sudo restorecon -Rv /var; ls -Z
```
Troubleshooting logs to service
```ruby
journalctl -n 20 (to check 20 lines)
journalctl -xeu rsyslog.service
sudo systemctl reset-failed rsyslog (if failed after reset too fast error)
```
## Resource
https://docs.aws.amazon.com/ebs/latest/userguide/ebs-using-volumes.html
