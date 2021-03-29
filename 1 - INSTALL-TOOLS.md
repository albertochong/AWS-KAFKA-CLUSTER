# Installing Tools and Packages in all instances
### Tools
* Install wget
* Install nano
* Install net-tools
* Install zip
* Install netcat
* Install tar
* Install Java
 
## Run in each terminal

* Install all 
```bash
sudo apt-get install wget nano net-tools zip netcat tar
```

* Create folder to Java and kafka Installation, zookeeper data and logs
```bash
sudo mkdir /opt/jdk
sudo mkdir /opt/kafka
sudo mkdir /opt/zookeeper
sudo mkdir /opt/zookeeper/data
sudo mkdir /opt/zookeeper/logs
```

* Give previleges to user
```bash
sudo chown -R ubuntu:ubuntu /opt/jdk
sudo chown -R ubuntu:ubuntu /opt/kafka
sudo chown -R ubuntu:ubuntu /opt/zookeeper
sudo chown -R ubuntu:ubuntu /opt/zookeeper/data
sudo chown -R ubuntu:ubuntu /opt/zookeeper/logs

```


## Install Java Virtual machine.Kafka needs jdk to work 
### Run in terminal

* Check if java is installed
```bash 
java -version 
or
sudo yum list java*
``` 

* Remove all openjdk packages
```bash 
sudo yum remove java*
``` 

* (Download Java) under home/user directory
```bash 
wget -c --content-disposition "https://javadl.oracle.com/webapps/download/AutoDL?BundleId=239835_230deb18db3e4014bb8e3e8324f81b43"
```

* Unpack 
```bash 
sudo tar xzf java_package_name
```

*  Move folder jdk1.8.0_221 to opt/jdk
```bash 
sudo mv jdk1.8.0_221/ /opt/jdk
```


*  Edit .bashrc Under home/user directory to create envirnoment variable for java and add
```bash 
nano .bashrc
  export JAVA_HOME=/opt/jdk
  export JRE_HOME=/opt/jdk/jre
  export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```

* Reactive and update and read news environment variable
```bash 
source .bashrc
```

* To confirm java installation
```bash 
java -version 
```    
