# AWS-KAFKA-CLUSTER
Create Messaging environment with Apache Kafka Multi broker on AWS machine


## Create 1 Instance (t2.Xlarge) with the options
* AMI: Amazon Linux 2 AMI (HVM), SSD Volume Type 64 bit
* Network: vpc by default + No precense subnet + Public IP Use subnet  setting (enable)
* Storage: Root disk 40 GB + Add Volume 200GB Magnetic (delete on termination)
* Use security group created (CslChongKafkaGroup)
* Create or use new .pem key (copy this file in a secure place)

## Security

### Create a Security Group

* Create new security group with the name: ClsChongNameNodeGroup
```bash
Type              Protocol        Port        Source

All Traffic       All             0 - 65535   Anywhere   

SSH               TCP             22          Anywhere
```

## 1. Step: Install some tools
  * Install wget
  * Install nano
  * Install SSH
  * Install Java

## 2. Step: Kafka Installation and Configuration
  * Install and Configure Kafka Multibroker 
  
### 2.1 Step: Working with KAFKA
  *  Create Topic
  * 
  *  
  

