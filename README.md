# AWS-KAFKA-CLUSTER
Create environment with Apache Kafka Multi broker on AWS machine with 3 zookeepers and 3 brokers in same zone.


## Create 6 Instances(3 Zookeepers, 3 borkers) (t2.Xlarge) with the options
* AMI: Amazon Ubuntu 18.0.4 TLS AMI (HVM), SSD Volume Type 64 bit
* Network: vpc by default + No precense subnet + Public IP Use subnet  setting (enable)
* Storage: Root disk 40 GB + Add Volume 200GB Magnetic (delete on termination)
* Use security group created (ChongGroup)
* Create or use new .pem key (copy this file in a secure place)

## Security

### Create a Security Group

* Create new security group with the name: ChongGroup
```bash
Type              Protocol        Port        Source    Description
All Traffic       All             0 - 65535   Anywhere   
SSH               TCP             22          Anywhere  Connection ssh
CUSTOM TCP        TCP             2181        Anywhere  Zookeeper
CUSTOM TCP        TCP             2888        Anywhere  Zookeeper
CUSTOM TCP        TCP             3888        Anywhere  Zookeeper
CUSTOM TCP        TCP             9092        Anywhere  Broker
CUSTOM TCP        TCP             8083        Anywhere  Connect
CUSTOM TCP        TCP             8088        Anywhere  Ksql
CUSTOM TCP        TCP             8000        Anywhere  Prometheus Agent Jmx
CUSTOM TCP        TCP             9090        Anywhere  Prometheus Server
CUSTOM TCP        TCP             3000        Anywhere  Grafana
```

## 1. Step: Install some tools
  * Install wget
  * Install nano
  * Install net-tools
  * Install zip
  * Install netcat
  * Install tar
  * Install Java

## 2. Step: Apache Kafka 2.7.0 Installation
  * Download Apache Kafka 2.7.0 
  * Create env variables
  
### 2.1 Step: Zookeeper Ensemble 3 nodes Configuration
  *  Mock and add dns entries to hosts file
  *  Create data dictionary for zookeeper and transaction log directory and give previleges
  *  Edit the zookeeper.properties and set main properties to get up the cluster
  *  Create the file Myid to identify the nodes
  *  Configure zookeeper as service
  *  Testing cluster comunication

### 2.2 Step: 3 brokers Configuration
  *  Format 2 differents drives for data
  *  Mock and add dns entries to hosts file
  *  Edit the server.properties and set main properties to get up the cluster
  *  Testing comunication between brokers and zookeeper
  *  Setup Broker as service

### 2.3 Step: KAFKA Tests basics operations
  *  Create,describe topic
  *  Testing producer messages to the topic

### 2.4 Step: KAFKA Cluster Security
  *  TLS in Kafka
  *  Generate a Certificate Authority (CA)
  
### 2.5 Step: KAFKA API Java Clients
  *  Create Producer and Consumer
  
## 3. Step: Kafka Monitoring
  * Install and Configuring Prometheus
  * Install and Configuring Grafana

