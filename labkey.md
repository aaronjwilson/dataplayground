#ubuntu server
Building from source code was tricky for me because i did not have a background in CLI or meshing tools together.  The online documentation was good but has been through a few iterations and there are tidbits left over from older versions that no longer apply.  Alas here is my version.  works on ubuntu server or a cloud server.  there is an ami but i wanted to learn how to do it.

the basics of what i will install are:
1. oracle java
2. apache tomcat
3. postgresql
4. labkey web application
5. R


1. order
2. list
3. test
4. three

Note: some of the code is outdated or written by myself a beginner.  Would like to at some point set this all up in a bash script for easier execution.

```
sudo mkdir /usr/local/java /usr/local/tomcat /usr/local/labkey
```

#create PATHS

```
sudo nano /etc/profile
```
Paste this at the end of the profile file
```
LABKEY_HOME=/usr/local/labkey
JAVA_HOME=/usr/local/java/jdk1.8.0
JRE_HOME=/usr/local/java/jdk1.8.0/jre
CATALINA_HOME=/usr/local/tomcat
CATALINA_OPTS=-Djava.awt.headless=true
PATH=$PATH:$HOME/bin:$LABKEY_HOME/bin
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
PATH=$PATH:$HOME/bin:$JRE_HOME/bin
PATH=$PATH:$HOME/bin:$CATALINA_HOME/bin
#alternatively can be one line
#PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$JRE_HOME/bin:$CATALINA_HOME/bin
export LABKEY_HOME
export JAVA_HOME
export JRE_HOME
export CATALINA_HOME
export PATH
```
Note: would like to create a sudo echo write for this information.
See appendix
echo "echo LABKEY_HOME=/usr/local/labkey >> /etc/profile" | sudo bash


#installing java jdk on ubuntu64bit
this should allow a bypass of accepting licensing queries.
Note: the version of the jdk will probably have changed
```
wget --no-cookies --no-check-certificate --header "Cookie:gpw_e24=http%3A%2F%2F%www.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u25-b17/jdk-8u25-linux-x64.tar.gz"
```

```
sudo chmod a+x jdk-8u25-linux-x64.tar.gz
sudo cp jdk-8u25-linux-x64.tar.gz /usr/local/java
cd /usr/local/java
sudo tar xzf jdk-8u25-linux-x64.tar.gz

sudo update-alternatives --install /usr/bin/java java /usr/local/java/jdk1.8.0_25/bin/java 1
sudo update-alternatives --config java
java -version
```

#Installing Apache TOMCAT
as of current labkey implementations there is no mention of TOMCAT v 8.0 in the [supported platforms](https://www.labkey.org/wiki/home/Documentation/page.view?name=supported#tomcat) 

```
#download the tar file
wget / http://mirror.olnevhost.net/pub/apache/tomcat/tomcat-7/v7.0.56/bin/apache-tomcat-7.0.57.tar.gz
```

unzip and install the file to the usr/local/tomcat folder
it is recommended to create a symbolic link to the version folder but i am just moving out all of the files into the tomcat folder that i made.
```
sudo chmod a+x apache-tomcat-7.0.57.tar.gz
sudo cp apache-tomcat-7.0.57.tar.gz /usr/local/tomcat
cd /usr/local/tomcat
sudo tar xzf apache-tomcat-7.0.57.tar.gz
sudo mv  bin/ conf/ lib/ logs/ temp/ webapps/ work/ $CATALINA_HOME
#to test the installation
sudo bin/startup.sh 
```
if all works if you migrate to your localhost you should see the apache webhost page.

#installing POSTGRESQL
it was a bit of a pain on the ami but reboots and updates with help solve this.  
```
sudo apt-get update
sudo apt-get install postgresql
```
configuring the database and setting a password
```
sudo su postgres
createuser -P labkey --superuser
#check rights
psql
\du
#may need to: ALTER ROLE labkey WITH REPLICATION LOGIN;
\q
```

#Labkey webapp installation
procure the most up to date version. (15 as of this writing) 

```
wget http://labkey.s3.amazonaws.com/downloads/general/r/14.3/LabKey14.3-35337.10-bin.tar.gz
```

