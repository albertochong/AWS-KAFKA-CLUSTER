# AWS-KAFKA-CLUSTER
Create Messaging environment with Apache Kafka Multi broker on AWS machine with 3 zookeepers and 3 brokers in same zone.


## Create 6 Instances (t2.Xlarge) with the options
* AMI: Amazon Ubuntu 18.0.4 TLS AMI (HVM), SSD Volume Type 64 bit
* Network: vpc by default + No precense subnet + Public IP Use subnet  setting (enable)
* Storage: Root disk 40 GB + Add Volume 200GB Magnetic (delete on termination)
* Use security group created (ChongGroup)
* Create or use new .pem key (copy this file in a secure place)

## Security

### Create a Security Group

* Create new security group with the name: ChongGroup
```bash
Type              Protocol        Port        Source
All Traffic       All             0 - 65535   Anywhere   
SSH               TCP             22          Anywhere
CUSTOM TCP        TCP             2181        Anywhere
CUSTOM TCP        TCP             2888        Anywhere
CUSTOM TCP        TCP             3888        Anywhere
CUSTOM TCP        TCP             9092        Anywhere
CUSTOM TCP        TCP             8083        Anywhere
CUSTOM TCP        TCP             8088        Anywhere
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
  *  Mock and add dns entries
  *  Create data dictionary for zookeeper and log directory
  *  

### 2.2 Step: Install and Configure 3 brokers
  *  XXXXXXXXXXX

### 2.3 Step: KAFKA Cluster Security
  *  XXXXXXXXXXX

### 2.4 Step: KAFKA Tests
  *  Create,describe topic
  *  Testing producer messages to the topic
  *  Testing consumer messages from the topic

### 2.5 Step: KAFKA API Java Clients
  *  Create Producer
  *  Create Consumer
  
## 3. Step: Kafka Monitoring
  * Install and Configuring Prometheus
  * Install and Configuring Grafana

