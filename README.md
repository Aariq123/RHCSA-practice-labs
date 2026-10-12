
* **Practice Source Material:** Questions adapted from the public (https://github.com/ive663/RHCSA) repository. 
* **Lab Environment:** All solutions were executed, verified, and thoroughly tested inside a virtualized **Rocky Linux 10** instance


## **Task 01**: Assuming the root user password is lost, and your system is running in multi-user target with no current root session open. Reboot the system into an appropriate target, and reset the root user password to root. Log in with root and perform the remaining tasks.


interrupt the boot process, press `e` to edit the grub config file
add `init=/bin/bash` at the end of the Linux kernel line, press **ctrl+x** to continue the boot process

`mount -o remount,rw /`    
`passwd`  
      enter new pasword twice  
`touch /.autorelabel`  
`exec /sbin/bash`  
  



## **Task 02**: Configure a network connection on the primary network device with IP address 192.168.0.241/24, gateway 192.168.0.1, and nameserver 192.168.0.1 (you may use different IP assignments based on your lab environment).
  
`nmcli connection add con-name static-config ifname ens192 type ethernet ip4 192.168.0.241/24 gw4 192.168.0.1 ipv4.dns 192.168.0.1 autoconnect no`  

  
## **Task 03**: Using a manual method (modify file by hand), set the system hostname to rhcsa1.example.com. Make sure that the new hostname is reflected in the command prompt.  
`nmcli general hostname rhcsa1.example.com`  
 


## **Task 04**: Set the default boot target to multi-user.   

`systemctl set-default multi-user.target`  

## **Task 05**: Set SELinux to permissive mode.   

`nano /etc/selinux/config`  
find the line **SELINUX=enforcing** and change it to **SELINUX=permissive**  

  
## Task 06: Perform a case-insensitive search for all lines in the /usr/share/dict/linux.words file that begin with the pattern “essential”. Redirect the output to /tmp/pattern.txt file. Make sure that empty lines are omitted.

`grep -i '^essential' /usr/share/dict/linux.words > /tmp/pattern.txt`    

  
## **Task 07**: Create user accounts called user10, user20, and user30. Set their passwords to Temp1234. Make accounts for user10 and user30 to expire on December 31, 2021.  

`useradd user10`  
`useradd user20`  
`useradd user30`  
`passwd user10`  enter passwd   
`passwd user20`  enter passwd      
`passwd user30`  enter passwd   
`chage -E "2021-12-31" user10`  
`chage -E "2021-12-30" user30`  
  
## **Task 08**: Create a user account called user40 with UID 2929. Set the password to user1234.  
`useradd user40 -u 2929`  
`passwd user40  `enter passwd  


## **Task 09**: Create a directory called dir1 under /tmp with ownership and owning group set to root. Configure default ACLs on the directory and give user user10 read, write, and execute permissions.  


`mkdir /tmp/dir1`  
`chown root:root /tmp/dir1`  
`setfacl -m u:user10:rwX /tmp/dir1`  
`setfacl -m d:u:user10:rwX /tmp/dir1`  
`getfacl /tmp/dir1`  
  
   


## Task 10: Create a logical volume called lvol1 of size 300MB in vgtest volume group. Mount the Ext4 file system persistently to /mnt/mnt1.  

`fdisk /dev/sdb`   assume we are dealing with gpt partition scheme
`fdisk` `n`  
`fdisk` first sector: press enter, last sector: +3G(Whatever space is available that's equal or greater than 300M)   
`fdisk p`  
`fdisk w` 
`pvcreate /dev/sdb1`
`vgcreate vgtest /dev/sdb1` 
`lvcreate -L 300m /dev/vgtest -n lvo1`  
`mkfs.ext4 /dev/vgtest/lvol1`  
test `blkid /dev/vgtest/lvol1`  
`mkdir /mnt/mnt1`  
`nano /etc/fstab`
UUID=(the lv uuid here)     /mnt/mnt1    ext4    defaults      0 0"   
`mount -a`
  


## **Task 11**: Change group membership on /mnt/mnt1 to group10. Set read/write/execute permissions on /mnt/mnt1 for group members, and revoke all permissions for public.  
  
`chown :group10 /mnt/mnt1`  
`chmod 770 /mnt/mnt1`  

## **Task 12**: Create a logical volume called lvswap of size 300MB in vgtest volume group. Initialize the logical volume for swap use. Use the UUID and place an entry for persistence.   
  

`lvcreate -n lvswap -L 300m vgtest`  
`mkswap /dev/vgtest/lvswap`  
`nano /etc/fstab` add this: "UUID=xxxx-xxxx-xxxx-xxxx-xxxx         swap       swap       defaults      0 0"

`swapon -a`  
`swapon --show`  
  

## **Task 13**: Use tar and bzip2 to create a compressed archive of the /etc/sysconfig directory. Store the archive under /tmp as etc.tar.bz2.  
  
`tar cfj /tmp/etc.tar.bz2 /etc/sysconfig`  
`tar tfv /tmp/etc.tar.bz2` #list the archive for the test  

  
