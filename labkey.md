#ubuntu server

'''
sudo mkdir /usr/local/java /usr/local/tomcat /usr/local/labkey
'''

#create PATHS
sudo nano /etc/profile

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


#installing java
