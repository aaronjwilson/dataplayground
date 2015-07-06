#ubuntu server
Building from source code was tricky for me because i did not have a background in CLI or meshing tools together.  The online documentation was good but has been through a few iterations and there are tidbits left over from older versions that no longer apply.  Alas here is my version.  works on ubuntu server or a cloud server.  there is an ami but i wanted to learn how to do it.  

the basics of what i will install are:  
1. java - from oracle not the openjdk
2. tomcat server
3. postgresql
4. labkey configuration
5. R
 

```
sudo mkdir /usr/local/java /usr/local/tomcat /usr/local/labkey
```

#create PATHS
```sudo nano /etc/profile

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

#installing java
