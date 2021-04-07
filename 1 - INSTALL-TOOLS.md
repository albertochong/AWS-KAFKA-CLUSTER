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

* Create folder to kafka Installation and give previleges to user
```bash
sudo mkdir /opt/kafka
sudo chown -R ubuntu:ubuntu /opt/kafka
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

* Install Java 
```bash 
sudo apt-get update && sudo apt-get install -y openjdk-8-jdk
```


* To confirm java installation
```bash 
java -version 
```    
