aws_manage_snapshots
====================
```
version: 1.3
usage: ./aws_manage_snapshots.bash [OPTIONS]
 -h  Show this message
 -t  List Volumes and which Machine they are attached to, don't take any action
 -s  Take a snapshot of all volumes listed by above action
 -v  List each clients attached volumes and their associated snapshots
 -d  Delete all but X most recent snapshots for each volume listed by above action
 -i  Write an inventory for each client to the log dir
 -l  Choose log dir
 -k  Choose key dir
 -c  Specify which detected accounts you wish to run the script against.
 -a  Specify which avaliablility zones you wish to run the script against.

Example Inventory mode :./aws_manage_snapshots.bash  -i -l /var/log/aws/ -k /etc/ssl/private/aws/
Example Snapshot mode  :./aws_manage_snapshots.bash  -s -l /var/log/aws/ -k /etc/ssl/private/aws/
Example Delete mode saving the 15 most recent snapshots  :./aws_manage_snapshots.bash  -d 15
Example List attached volumes for client foo in zone us-east-1:./aws_manage_snapshots.bash -v -c foo -a us-east-1
Note: keys must be in the format projectname.key and projectname.pub

zones: eu-west-1 sa-east-1 us-east-1 ap-northeast-1 us-west-2 us-west-1 ap-southeast-1 ap-southeast-2

detected accounts:
foo 
bar
buzz
```

Requirements
============
ec2-api-tools  -- avaliable in ubntu 12.04 multiverse repo

```
ec2-api-tools
 |Depends: default-jre-headless
  Depends: <java6-runtime-headless>
    openjdk-6-jre-headless
    default-jre-headless
    openjdk-7-jre-headless

```

INSTALLING IN DEBIAN
====================


Get *shudder* java
```
apt-get install default-jre-headless unzip git
export JAVA_HOME="/usr/lib/jvm/java-6-openjdk-amd64/"

:~# $JAVA_HOME/bin/java -version
java version "1.6.0_27"
OpenJDK Runtime Environment (IcedTea6 1.12.6) (6b27-1.12.6-1~deb7u1)
OpenJDK 64-Bit Server VM (build 20.0-b12, mixed mode)

```

Get the ec2-api-tools

```
wget http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip && unzip ec2-api-tools.zip
mkdir /opt/ec2api/ && mv ec2-api-tools-1.6.12.0/bin/ /opt/ec2api/ && mv ec2-api-tools-1.6.12.0/lib/ /opt/ec2api/

export EC2_HOME=/opt/ec2api/
export PATH=$PATH:$EC2_HOME/bin
export EC2_URL=http://ec2.amazonaws.com
```

Get this script
```
cd /opt/
git clone clone https://github.com/enovance/aws_manage_snapshots.git 
mkdir /etc/ssl/private/aws/

#mv your key pairs into the key dir (default /etc/ssl/private/aws/)

:/opt/aws_manage_snapshots# ./aws_manage_snapshots.bash -t
2013/11/29 16:04:00: running ec2-describe-instances to list enovance's volumes and attachements in eu-west-1 avaliablity zone
...
[/snip]
```

Dont forget to update /etc/profile or .bashrc
````
export JAVA_HOME="/usr/lib/jvm/java-6-openjdk-amd64/"
export EC2_HOME=/opt/ec2api/
export PATH=$PATH:$EC2_HOME/bin
export EC2_URL=http://ec2.amazonaws.com
```



