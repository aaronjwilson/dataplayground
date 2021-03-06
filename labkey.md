#ubuntu server
Building from source code was tricky for me because i did not have a background in CLI or meshing tools together.  The online documentation was good but has been through a few iterations and there are tidbits left over from older versions that no longer apply.  Alas here is my version.  works very well on Ubuntu server or AWS cloud server.  There is a premade AMI but i needed to learn how to do it for my own edification.

the basics of what i will install are:

1. Oracel Java
2. Apache Tomcat
3. Postgresql
4. Labkey Web Application
5. R

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
if all works you can type in http//localhost:8080 and will see the apache web server page.  

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
```
We will need to create a password for the labkey user profile. 
We will need to check the permissions of the labkey account and make some additional alterations
```
psql
\du
#in my experience the labkey account is lacking in to areas replication and login.  we will add those roles.
ALTER ROLE labkey WITH REPLICATION LOGIN;
\q
```
I am still learning the navigation and implementation of Postgres but wanted to focus on getting the server up and running to play with data: relegated to bucket list. 

#Labkey webapp installation
procure the most up to date version from the Labkey.com website. (version 15 as of this writing) 

```
wget http://labkey.s3.amazonaws.com/downloads/general/r/15.2/LabKey15.2-bin.tar.gz
```
chmod. unzip and install
```
sudo chmod a+x LabKey
sudo tar xzf LabKey15.2-bin.tar.gz
```
Next we need to move some libraries and files into requisite folders.  we will do this with the wildcard (*.file extension).

```
sudo cp -r LabKey15.2-bin/tomcat-lib/*.jar $CATALINA_HOME/lib
sudo cp -r LabKey15.2-bin/{bin,labkeywebapp,modules,pipeline-lib} /usr/local/labkey
sudo cp LabKey15.2-bin/labkey.xml $CATALINA_HOME/conf/Catalina/localhost
```
Now that all the files are in there respective places we will add in some additional information regarding the login to the account
```
sudo nano $CATALINA_HOME/conf/Catalina/localhost/labkey.xml
```
Information needs to be added where the @@ are located
```
@@appdocbase@@  will become /usr/local/labkey/labkeywebapp or $LABKEY_HOME/labkeywebapp 
@@jdbcUser@@ will become your Postgres user login:  labkey
@@jdbcPassword@@ will become your password for the Postgres labkey user profile
```
And finally there will be some memory optimzations for Java (which I do not understand, but it works)
```
sudo nano $CATALINA_HOME/bin/catalina.sh
#place the following code just above the line that says " #OS specific support
JAVA_OPTS="$JAVA_OPTS -Xms128m -Xmx2048m -XX:MaxPermSize=256M -XX:-HeapDumpOnOutOfMemoryError"
```
And we are done
reboot your machine and when it comes back online type
```
sudo $CATALINA_HOME/bin/startup.sh
```
and navigate to localhost:8080/labkey and the server should start.  you will need to load some modules but it is pretty self explanatory from there. 
Then build your data aggregation lab site

NOTE:  if you host online many ISPs will not support 8080 as a port (AWS will support it). 
To change this go to 
```
sudo nano $CATALINA_HOME/conf/server.xml
```
and change port 8080 where it says
```
<Connector port="8080" protocol="HTTP/1.1"
```
