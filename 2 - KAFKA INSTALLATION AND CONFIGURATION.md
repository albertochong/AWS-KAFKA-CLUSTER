
# KAFKA MultiBroker Installation and Configuration
Cluster with 2 brokers in same server

### Run in terminal 

* Download Kafka under home/hadoop directory
```bash
wget https://archive.apache.org/dist/kafka/1.1.0/kafka_2.11-1.1.0.tgz
```

* unpack files
```bash
tar -xvf kafka_2.11-1.1.0.tgz
```

* move to folder opt/kafka 
```bash
sudo mv kafka_2.11-1.1.0/ /opt/kafka 
```

* Add kafka envirnoment variables under home/hadoop
```bash  
nano .bash_profile
 
# KAFKA
export KAFKA_HOME=/opt/kafka
export PATH=$PATH:$KAFKA_HOME/bin
```     

* Renitialize/Activate
```bash   
source .bash_profile
``` 

* Check if folder zookeeper .jar is there means zookeeper is already installed
```bash   
cd /opt/kafka/libs
``` 

* Create broker 1.Go to <KAFKA_HOME> installation and config folder
```bash   
cp  server1.properties  server1.properties
``` 

* Edit Broker 0.Go to <KAFKA_HOME> installation and config folder.
```bash   
nano server.properties
listeners=PLAINTEXT://ec2-3-17-3-207.us-east-2.compute.amazonaws.com:9092
log.dir=/tmp/kafka-logs
zookeeper.connect=localhost:2181
``` 

* Edit Broker 1.Go to <KAFKA_HOME> installation and config folder.
```bash   
nano server1.properties
broker.id=1
listeners=PLAINTEXT://ec2-3-17-3-207.us-east-2.compute.amazonaws.com:9093
log.dir=/tmp/kafka-logs1
zookeeper.connect=localhost:2181
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
