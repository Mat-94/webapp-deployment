# webapp-deployment
DEPLOYING WEBAPP THROUGH TOMCAT
after i built webapp using maven, i need to deploy through tomcat.

Step 1:Tomcat Installation:
#!/bin/bash
# install Java JDK 1.8+ as a pre-requisite for tomcat to run.
# EC2 t2micro Ubuntu 20.04
# Install Prerequisites/Requirements
sudo apt update
 sudo apt install openjdk-11-jdk wget -y

Step 2: # Download tomcat software and extract it.
VERSION=9.0.97
sudo wget https://downloads.apache.org/tomcat/tomcat-9/v${VERSION}/bin/apache-tomcat-${VERSION}.tar.gz
sudo tar -xzvf apache-tomcat-${VERSION}.tar.gz
sudo rm apache-tomcat-${VERSION}.tar.gz
sudo mv apache-tomcat-${VERSION} tomcat9

 step 3: starting of tomact
 sudo chmod 777 -R /opt/tomcat9
sudo sh /opt/tomcat9/bin/startup.sh

step 4:
# create a soft link to start and stop tomcat
sudo ln -s /opt/tomcat9/bin/startup.sh /usr/bin/starttomcat
sudo ln -s /opt/tomcat9/bin/shutdown.sh /usr/bin/stoptomcat
sudo starttomcat
echo "end on tomcat installation"

step 5:Tomcat Configuration:
sudo vi /opt/tomcat9/webapps/manager/META-INF/context.xml
comment this out

<!--
   <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
step 6:
How to create Tomcat Users:
sudo vi /opt/tomcat9/conf/tomcat-users.xml

 <user username="<admin>" password="<password-to-change>" roles="manager-gui"/>

 <user username="jomacs" password="admin123" roles="manager-gui,admin-gui,manager-script"/>
 <user username="femi" password="admin123" roles="manager-gui"/>

Step 7:
sudo stoptomcat
sudo starttomcat    

THE IMAGE BELOW SHOWS WEBAPP IS NOT YET DEPLOYED

![Screenshot (22)](https://github.com/user-attachments/assets/92bd3563-5b98-4241-b270-f8b1307b300a)


Step 8 : 
 Enable password Authentication:
  sudo vi /etc/ssh/sshd_config
  PasswordAuthentication=yes 

  or 
  sudo sed -i "/^[^#]*PasswordAuthentication[[:space:]]no/c\PasswordAuthentication yes" /etc/ssh/sshd_config

step 9: Restart service:
sudo systemctl restart sshd
or
sudo service sshd restart

step 10: Default Port: 8080 

sudo vi /opt/tomcat9/conf/server.xml

Look for 
  <Connector port="8080" protocol="HTTP/1.1"

STEP 11: Deploying 

Deploy into Tomcat:

Maven Home Directory = /opt/maven
Tomcat Home Directory = /opt/tomcat9

Tomcat Depolyment Directory = /opt/tomcat9/webapps

Copy web-app.war within same server:
cp <path-to-source> <path-to-destination>
cp web-app.war /opt/tomcat9/webapps

Copy web-app.war from Maven server to Tomcat server:
scp <path-to-source> user@ip:<path-to-destination>
scp web-app.war ubuntu@ip:/opt/tomcat9/webapps
scp web-app.war ec2-user@ip:/opt/tomcat9/webapps

ssh -i <path-to-key> user@ip

scp web-app.war ec2-user@18.191.157.132:/opt/tomcat9/webapps/jomacs.war

IMAGE BELOW SHOWS A SUCCESSFUL DEPLOYMENT
![Screenshot (26)](https://github.com/user-attachments/assets/a4420296-9434-4115-b5c8-6eb3f71d38da)


scp web-app.war ec2-user@18.191.157.132:/opt/tomcat9/webapps/competitors.war
BY CLICKING ON THE WEBAPP/ LINK THE IMAGE BELOW SHOWED,
![Screenshot (27)](https://github.com/user-attachments/assets/ac653331-1ddf-499d-bab7-649740fc6e84)





  













