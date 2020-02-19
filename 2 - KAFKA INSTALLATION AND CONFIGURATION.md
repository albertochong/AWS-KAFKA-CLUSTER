
# KAFKA Multi Broker Installation and Configuration
Server with 4 brokers

### Run in terminal 

* Download Kafka under home/hadoop directory
```bash
wget https://archive.apache.org/dist/kafka/1.1.0/kafka_2.11-1.1.0.tgz
```

* unpack files
```bash
tar -xvf kafka_2.11-1.1.0.tgz
```

* move to folder opt/kafka and change ownner
```bash
sudo mv kafka_2.11-1.1.0/ /opt/kafka 
sudo chown -R hadoop:hadoop /opt/kafka/
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

* Create broke 0, broker 1 and broker 2.Go to KAFKA folder installation and config folder
```bash   
cp  server.properties  server1.properties
cp  server.properties  server2.properties
cp  server.properties  server3.properties
``` 

* Edit Broker 1.Go to KAFKA folder installation and config folder.
```bash   
nano server1.properties
broker.id=1
listeners=PLAINTEXT://ec2-3-17-3-207.us-east-2.compute.amazonaws.com:9093
``` 

* Edit Broker 2.Go to KAFKA folder installation and config folder.
```bash   
nano server1.properties
broker.id=2
listeners=PLAINTEXT://ec2-3-17-3-207.us-east-2.compute.amazonaws.com:9094
``` 

* Edit Broker 3.Go to KAFKA folder installation and config folder.
```bash   
nano server1.properties
broker.id=3
listeners=PLAINTEXT://ec2-3-17-3-207.us-east-2.compute.amazonaws.com:9095
``` 

* Putting  zookeeper(kafka is managed by zookeeper) running background.Go to KAFKA folder installation 
```bash 
nohup bin/zookeeper-server-start.sh config/zookeeper.properties > logs/zookeeper.log &
tail -f logs/zookeeper.log (show lasts lines)
``` 

* Putting all brokers running background. Go to KAFKA folder installation
```bash 
nohup bin/kafka-server-start.sh config/server.properties > logs/broker.log &
nohup bin/kafka-server-start.sh config/server1.properties > logs/broker1.log &
nohup bin/kafka-server-start.sh config/server2.properties > logs/broker2.log &
nohup bin/kafka-server-start.sh config/server3.properties > logs/broker3.log &
tail -f logs/broker1.log (show lasts lines)
tail -f logs/broker2.log (show lasts lines)
tail -f logs/broker2.log (show lasts lines)
``` 
