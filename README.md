# Data-science

Notes for configuring AWS for use with various data science tools on Ubuntu trusty installation.


## Rstudio Server
Follow the installation steps on the RStudio Server website. In particular note the section regarding public keys to access the repositories. This must be done before the install or upgrade commandes will work.

Expose the Rstudio server ports by modifying the AWS security group that the instance sits on.

Create at least one valid user for RStudio server using the normal approach for <a href=http://linux.die.net/man/8/useradd> linux</a>.
Note that it is important for the user to be given a home directory or logging in to RStudio server will fail. 

```bash
#Create the user with a home directory
$sudo useradd -d /home/[username] -m [username]

#create a password for the user
$sudo passwd [username]
```

### Increase the RAM for RStudio server or increase swapfile
These instructions were necessary to increase memory and get around issues with RStudio being unable to allocate sufficent memory to install knitr or load certain packages. Instructions are based on the instructions found here:

https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-12-04
```bash

 # check to see if there is an existing swap
 $ sudo swapon -s

 # make sure we have enough disk space free, need at least 256k
 $ df -h

 # make the swap.  this will only last until the machine is rebooted
 $ sudo /bin/dd if=/dev/zero of=/swapfile bs=1024 count=256k
 $ sudo /sbin/mkswap /swapfile

 # results in:
 # Setting up swapspace version 1, size = 262140 KiB
 # no label, UUID=2193e5eb-0482-420c-9a7d-53558084fd06

 $ sudo /sbin/swapon /swapfile

 # confirm that you can see it:
 $ swapon -s

 # To turn off the swap, run:
 $ sudo /sbin/swapoff /swapfile

 # To make it use this swap every time the machine is started
 # Add this to /etc/fstab:
  /swapfile swap swap defaults 0 0
 
 $ sudo chmod 666 /etc/fstab
 $ vim /etc/fstab
 
 
```

This wasn't enough to install knitr, so I increased it to 2GB of swap,
then it worked.

```bash

 # To resize the swap to 1GB instead of 256Mb
 $ sudo /sbin/swapoff /swapfile
 $ sudo rm /swapfile
 $ sudo /bin/dd if=/dev/zero of=/swapfile bs=1M count=1024
 1024+0 records in
 1024+0 records out
 1073741824 bytes (1.1 GB) copied, 29.6197 s, 36.3 MB/s
 $ sudo /sbin/mkswap /swapfile
 Setting up swapspace version 1, size = 1048572 KiB
 no label, UUID=17f490d4-4188-4eaa-84aa-3ea0fd62bfe4
 $ sudo swapon /swapfile

```
Swappiness in the file should be set to 10. Skipping this step may cause both poor performance, whereas setting it to 10 will cause swap to act as an emergency buffer, preventing out-of-memory crashes.

You can do this with the following commands:
```bash
$ echo 10 | sudo tee /proc/sys/vm/swappiness
$ echo vm.swappiness = 10 | sudo tee -a /etc/sysctl.conf
```
To prevent the file from being world-readable, you should set up the correct permissions on the swap file:
```bash
$ sudo chown root:root /swapfile 
$ sudo chmod 0600 /swapfile
```
