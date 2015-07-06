#ubuntu server
Building from source code was tricky for me because i did not have a background in CLI or meshing tools together.  The online documentation was good but has been through a few iterations and there are tidbits left over from older versions that no longer apply.  Alas here is my version.  works on ubuntu server or a cloud server.  there is an ami but i wanted to learn how to do it.

the basics of what i will install are:  
1. java - from oracle not the openjdk
2. tomcat server
3. postgresql
4. labkey configuration
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
#PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$JRE_HOME/bin:$CATALINA_HOME/bin
export LABKEY_HOME
export JAVA_HOME
export JRE_HOME
export CATALINA_HOME
export PATH
```

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