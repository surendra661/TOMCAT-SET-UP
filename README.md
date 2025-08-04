#!/bin/bash
set -e

# 1. Install Java 17
sudo apt update
sudo apt install -y openjdk-17-jdk

# 2. Set JAVA_HOME
echo "export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64" | sudo tee /etc/profile.d/java_home.sh
sudo chmod +x /etc/profile.d/java_home.sh
source /etc/profile.d/java_home.sh

# 3. Download and set up Tomcat
cd /opt
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.82/bin/apache-tomcat-9.0.82.tar.gz
sudo tar -xzf apache-tomcat-9.0.82.tar.gz
sudo mv apache-tomcat-9.0.82 tomcat9
sudo chmod +x /opt/tomcat9/bin/*.sh

# 4. Create admin user
sudo sed -i '/<\/tomcat-users>/i\
<role rolename="manager-gui"/>\n<user username="admin" password="admin" roles="manager-gui"/>' /opt/tomcat9/conf/tomcat-users.xml

# 5. Allow web access to manager (remove IP restrictions)
sudo sed -i 's/<Valve.*RemoteAddr.*\/>//' /opt/tomcat9/webapps/manager/META-INF/context.xml

# 6. Start Tomcat
sudo /opt/tomcat9/bin/startup.sh
