
# Apache Kafka 2.7.0 Installation
Cluster with 3 brokers and 3 zookeepers.

### Run in terminal on each 6 instances 
###### References : https://kafka.apache.org/downloads

* Download Kafka 
```bash
sudo wget https://archive.apache.org/dist/kafka/2.7.0/kafka_2.12-2.7.0.tgz
```

* unpack files
```bash
tar -xvzf kafka_2.12-2.7.0.tgz
```

* move to folder opt/kafka 
```bash
sudo mv kafka_2.12-2.7.0/ /opt/kafka 
```

* Add kafka envirnoment variables 
```bash  
nano .bashrc
 
# Kafka 
export KAFKA_HOME=/opt/kafka
export PATH=$PATH:$KAFKA_HOME/bin
```     

* Renitialize/Activate
```bash   
source .bashrc
``` 
