#!/bin/bash
set -e

# Install Java 17
sudo apt update
sudo apt upgrade -y
sudo apt install -y openjdk-17-jdk

# Set JAVA_HOME
echo "export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac))))" | sudo tee /etc/profile.d/java_home.sh
source /etc/profile.d/java_home.sh

# Download and install Tomcat 9
cd /opt
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.82/bin/apache-tomcat-9.0.82.tar.gz
sudo tar -xzf apache-tomcat-9.0.82.tar.gz
sudo mv apache-tomcat-9.0.82 tomcat9
sudo chmod +x tomcat9/bin/*.sh

# Add Tomcat admin user
sudo sed -i '/<\/tomcat-users>/i\
<role rolename="manager-gui"/>\n<user username="admin" password="admin" roles="manager-gui"/>' /opt/tomcat9/conf/tomcat-users.xml

# Allow remote access to manager GUI
sudo sed -i 's/<Valve.*RemoteAddr.*\/>//' /opt/tomcat9/webapps/manager/META-INF/context.xml

# Start Tomcat
sudo /opt/tomcat9/bin/startup.sh
