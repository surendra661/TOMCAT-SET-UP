sudo apt update
sudo upgrade -y
sudo apt install openjdk-17-jdk -y
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.82/bin/apache-tomcat-9.0.82.tar.gz
sudo tar -xvzf apache-tomcat-9.0.82.tar.gz
sudo mv apache-tomcat-9.0.82 tomcat9
sed -i '46 <role rolename="manager-gui"/>' /opt/tomcat9/conf/tomcat-users.xml
sed -i '47 <role rolename="manager-script"/>' /opt/tomcat9/conf/tomcat-users.xml
sed -i '48 <user username="tomcat" password="admin123" roles="manager-gui, manager-script"/> /opt/tomcat9/conf/tomcat-users.xml
sed -i '19d' /opt/tomcat9/webapps/manager/META-INF/context.xml
sed -i '20d' /opt/tomcat9/webapps/manager/META-INF/context.xml
sudo /opt/tomcat9/bin/shutdown.sh
sudo /opt/tomcat9/bin/startup.sh
