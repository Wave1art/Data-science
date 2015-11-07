#Configuring Mongo DB on AWS

##1. Create AWS Ubuntu instance

Ububtu trusty 14....


##2. Update all packages on Ubuntu

SSH onto the Ubuntu machine.

``` bash
$ sudo apt-get update
some code
```

The following steps are taken and modified from the Mongo help file here:
https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

```bash
#Import dependencies
$ sudo apt-get install libgssapi-krb5-2 libsasl2-2 libssl1.0.0 libstdc++6 snmp

#import public key used by Mongo to sign packages
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

#download 3.2 release candidate RC2 from mongo servers
$ cd ~
$ mkdir temp
$ cd temp/
$ curl -O http://downloads.10gen.com/linux/mongodb-linux-x86_64-enterprise-ubuntu1404-3.2.0-rc2.tgz

#expand compressed file
$ tar -zxvf mongodb-linux-x86_64-enterprise-ubuntu1404-3.2.0-rc2.tgz

#Copy extracted archive to target directory
$ sudo mkdir -p /home/mongodb
$ sudo cp -R -n /home/ubuntu/mongodb-linux-x86_64-enterprise-ubuntu1404-3.2.0-rc2/* /home/mongodb

```
Now ensure the location of the binaries is in the PATH variable
```bash
$ cd ~
$ vim .bashrc

#Add this to the bottom of the bash profile and save
export PATH=/home/mongodb/bin:$PATH
```
Create a data dictionary. Here we use the default approach and create a /data/db dictionary
```bash
$ sudo mkdir -p /data/db

#Change permissions on directory to allow read write for Mongo user
sudo chmod -R 777 /data
```
Set Mongo running using 
```bash
mongod
```
