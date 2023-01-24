WildFly (JBoss) Server on Ubuntu:
---------------------------------

Step 1: Install Java JDK on Ubuntu
-------
sudo apt update -y
sudo apt install openjdk-11-jdk -y
java --version

Step 2: Download WildFly Installation archive
-------           
Install curl and wget tools:
sudo apt install curl wget

Here we will download WildFly 26.x.
#WILDFLY_RELEASE=$(curl -s https://api.github.com/repos/wildfly/wildfly/releases/latest|grep tag_name|cut -d '"' -f 4)
#wget https://github.com/wildfly/wildfly/releases/download/${WILDFLY_RELEASE}/wildfly-${WILDFLY_RELEASE}.tar.gz

wget https://github.com/wildfly/wildfly/releases/download/27.0.1.Final/wildfly-27.0.1.Final.tar.gz

Once the file is downloaded, extract it.
tar xvf wildfly-${WILDFLY_RELEASE}.tar.gz

Move resulting folder to /opt/wildfly.
sudo mv wildfly-${WILDFLY_RELEASE} /opt/wildfly

Step 3: Configure Systemd for WildFly
-------
Let’s now create a system user and group that will run WildFly service.
sudo groupadd --system wildfly
sudo useradd -s /sbin/nologin --system -d /opt/wildfly  -g wildfly wildfly

Create WildFly configurations directory.
sudo mkdir /etc/wildfly

Copy WildFly systemd service, configuration file and start scripts templates from the /opt/wildfly/docs/contrib/scripts/systemd/ directory.
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/wildfly.conf /etc/wildfly/
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/wildfly.service /etc/systemd/system/
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/launch.sh /opt/wildfly/bin/
sudo chmod +x /opt/wildfly/bin/launch.sh

Set /opt/wildfly permissions.
sudo chown -R wildfly:wildfly /opt/wildfly

Reload systemd service.
sudo systemctl daemon-reload

Start and enable WildFly service:
sudo systemctl start wildfly
sudo systemctl enable wildfly

Confirm WildFly Application Server status.
sudo systemctl status wildfly

Here WildFly Appication page can be accessed using public I.P address and 8080 port.






WildFly (JBoss) Server on CentOS 7:
-----------------------------------

Step 1: Install Java JDK
-------
sudo yum install java-11-openjdk-devel
java --version

Step 2: Download WildFly Release archive
-------
Here we will download WildFly 26.1.0.Final.
sudo yum -y install wget curl
WILDFLY_RELEASE=$(curl -s https://api.github.com/repos/wildfly/wildfly/releases/latest|grep tag_name|cut -d '"' -f 4)
wget https://github.com/wildfly/wildfly/releases/download/${WILDFLY_RELEASE}/wildfly-${WILDFLY_RELEASE}.tar.gz

Once the file is downloaded, extract it.
tar xvf wildfly-${WILDFLY_RELEASE}.tar.gz

Move resulting folder to /opt/wildfly.
sudo mv wildfly-${WILDFLY_RELEASE} /opt/wildfly

Step 3: Configure Systemd for WildFly
-------
sudo groupadd --system wildfly
sudo useradd -s /sbin/nologin --system -d /opt/wildfly  -g wildfly wildfly

Create WildFly configurations directory.
sudo mkdir /etc/wildfly

Copy WildFly systemd service, configuration file and start scripts templates from the /opt/wildfly/docs/contrib/scripts/systemd/ directory.
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/wildfly.conf /etc/wildfly/
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/wildfly.service /etc/systemd/system/
sudo cp /opt/wildfly/docs/contrib/scripts/systemd/launch.sh /opt/wildfly/bin/
sudo chmod +x /opt/wildfly/bin/launch.sh

Set /opt/wildfly permissions.
sudo chown -R wildfly:wildfly /opt/wildfly

Reload systemd service.
sudo systemctl daemon-reload

Configure SELinux:
sudo semanage fcontext  -a -t bin_t  "/opt/wildfly/bin(/.*)?"
sudo restorecon -Rv /opt/wildfly/bin/

Start and enable WildFly service:
sudo systemctl start wildfly
sudo systemctl enable wildfly

Confirm WildFly Application Server status.
systemctl status wildfly