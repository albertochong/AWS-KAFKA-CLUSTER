
# Apache Kafka 2.7.0 MultiBroker Installation and Configuration
Cluster with 3 brokers.

### Run in terminal on each 6 instances 

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

*  edit the hosts file.Add the following to the hosts entries on all servers. 
```bash   
sudo nano /etc/hosts
172.31.22.104 zookeeper1
172.31.31.206 zookeeper2
172.31.22.176 zookeeper3
``` 









* Putting  zookeeper(kafka is managed by zookeeper) running background.Go to<KAFKA_HOME> installation 
```bash 
nohup bin/zookeeper-server-start.sh config/zookeeper.properties > logs/zookeeper.log &
tail -f logs/zookeeper.log (show lasts lines)
``` 

* Putting all brokers running background. Go to <KAFKA_HOME> installation
```bash 
nohup bin/kafka-server-start.sh config/server.properties > logs/broker.log &
nohup bin/kafka-server-start.sh config/server1.properties > logs/broker1.log &
tail -f logs/broker.log (show lasts lines)
tail -f logs/broker1.log (show lasts lines)
``` 
