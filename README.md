#!/bin/bash

# Java 17 install
sudo apt update -y
sudo apt install openjdk-17-jdk -y

# Tomcat download & extract
cd /opt
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.82/bin/apache-tomcat-9.0.82.tar.gz
sudo tar -xvzf apache-tomcat-9.0.82.tar.gz
sudo mv apache-tomcat-9.0.82 tomcat9
sudo chmod +x /opt/tomcat9/bin/*.sh

# User add (manager-gui access)
sudo sed -i '$i\<role rolename="manager-gui"/>' /opt/tomcat9/conf/tomcat-users.xml
sudo sed -i '$i\<role rolename="manager-script"/>' /opt/tomcat9/conf/tomcat-users.xml
sudo sed -i '$i\<user username="tomcat" password="admin123" roles="manager-gui,manager-script"/>' /opt/tomcat9/conf/tomcat-users.xml

# Restriction remove from manager app
sudo sed -i 's/<Valve /<!-- <Valve /' /opt/tomcat9/webapps/manager/META-INF/context.xml
sudo sed -i 's/\/>$/\/> -->/' /opt/tomcat9/webapps/manager/META-INF/context.xml

# Start tomcat
sudo /opt/tomcat9/bin/startup.sh
