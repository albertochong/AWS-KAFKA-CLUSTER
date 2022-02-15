

# Kafka Monitoring log, metrics with Elasticsearch and beats
Our example setup consists of the three-node Kafka cluster. Each node runs Kafka, along with Filebeat and Metricbeat to monitor the node. The Beats are configured via Cloud ID to send data to our Elasticsearch Service cluster. The Kafka modules shipped with Filebeat and Metricbeat will set up dashboards within Kibana for visualisation. As a note, if you want to try this in your own cluster, you can spin up a 14-day free trial of the Elasticsearch Service.

### Install and Configuring filebeat and metricbeat
###### References : https://www.elastic.co/blog/monitoring-kafka-with-elasticsearch-kibana-and-beats

                    
#### Run in terminal on each broker instance

* Download and Install Filebeat and Metricbeat Agent via the APT repository.
```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt-get update
sudo apt-get install filebeat metricbeat
systemctl enable filebeat.service
systemctl enable metricbeat.service
```

* Edit kafka service and add line to point to java agent
```bash
![alt text](https://chongtech.blob.core.windows.net/chongtechgithubimages/elastic_cloudid.PNG)

```

* Testing and checking mettrics
```bash
curl broker1:8000
curl broker2:8000 
curl broker3:8000 
```
![alt text](https://achong.blob.core.windows.net/gitimages/prometheus.PNG)


#### Run in terminal on each zookeeper instance

* Download and Install JMX Export Agent 
```bash
mkdir prometheus
cd prometheus
wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.15.0/jmx_prometheus_javaagent-0.15.0.jar
wget https://raw.githubusercontent.com/prometheus/jmx_exporter/master/example_configs/zookeeper.yaml
```

* Edit Zookeeper service and add line to point to java agent
```bash
sudo nano /etc/systemd/system/zookeeper.service
Environment="EXTRA_ARGS=-javaagent:/home/ubuntu/prometheus/jmx_prometheus_javaagent-0.15.0.jar=7000:/home/ubuntu/prometheus/zookeeper.yaml"

sudo systemctl daemon-reload
sudo systemctl restart zookeeper

```

* Testing and checking mettrics
```bash
curl zookeeper1:7000
curl zookeeper2:7000 
curl zookeeper3:7000 
```



#### Run in terminal on another machine to install and configure Prometheus Server
###### References : https://prometheus.io/

* Edit the hosts file and add the following entries 
```bash
sudo nano /etc/hosts
172.31.22.104 zookeeper1
172.31.28.226 zookeeper2
172.31.22.176 zookeeper3

172.31.3.111  broker1
172.31.5.198  broker2
172.31.10.181 broker3
```

* Download and install prometheus Server as service
```bash
# testing from this machine if we can get data from agent/exporter installed on the broker and zookeeeper 
curl broker1:8000 
curl broker2:8000
curl broker3:8000

curl zookeeper1:7000
curl zookeeper2:7000
curl zookeeper3:7000

# Downlaod
wget https://github.com/prometheus/prometheus/releases/download/v2.12.0/prometheus-2.12.0.linux-amd64.tar.gz

# Unpack
sudo tar -zxvf prometheus-2.12.0.linux-amd64.tar.gz /opt/

# Edit configuration an add lines
sudo nano /opt/prometheus-2.12.0.linux-amd64/prometheus.yml
################################################################################################


scrape_configs:
  - job_name: 'kafka'
    static_configs:
    - targets: ['broker1:8000', 'broker2:8000', 'broker3:8000'] #brokers

  - job_name: 'zookeeper'
    static_configs:
    - targets: ['zookeeper1:7000', 'zookeeper2:7000', 'zookeeper3:7000'] #zookeepers

###############################################################################################

# Setup Prometheus as service
sudo nano /etc/systemd/system/prometheus.service
###############################################################################################################

[Service]
Type=simple
User=root
Group=root
ExecStart=/opt/prometheus-2.12.0.linux-amd64/prometheus --config.file=/opt/prometheus-2.12.0.linux-amd64/prometheus.yml --storage.tsdb.path=/op$
LimitNOFILE=65000
Restart=always

[Install]
WantedBy=multi-user.target

###############################################################################################################
```

* Reload, enable and start the service
```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus
```
![alt text](https://achong.blob.core.windows.net/gitimages/prometheus_running.PNG)

##### WEB : http://18.144.123.83:9090



### Install and Configure Grafana
#### Run in terminal on another machine to install and configure Grafana
###### References : https://grafana.com/


* Download and install grafana Server as service
```bash

# Downlaod
wget https://dl.grafana.com/oss/release/grafana-7.5.2.linux-amd64.tar.gz

# Unpack
sudo tar -zxvf grafana-7.5.2.linux-amd64.tar.gz /opt/


# Setup Grafana as service
sudo nano /etc/systemd/system/grafana.service
###############################################################################################################

[Service]
Type=simple
User=ubuntu
Group=ubuntu
WorkingDirectory=/opt/grafana-7.5.2
ExecStart=/opt/grafana-7.5.2/bin/grafana-server

[Install]
WantedBy=multi-user.target

###############################################################################################################
```

* Reload, enable and start the service
```bash
sudo systemctl daemon-reload
sudo systemctl start grafana
sudo systemctl status grafana
```
![alt text](https://achong.blob.core.windows.net/gitimages/grafana.PNG)


* Add prometheus as datasource in grafana
![alt text](https://achong.blob.core.windows.net/gitimages/grafana_add_datasource.PNG)

### Metrics
**CPU, JVM MEMORY, MESSAGES IN PER TOPIC, BYTES IN PER TOPIC, BYTES OUT PER TOPIC**
```bash
 1 - Import from https://grafana.com/grafana/dashboards/721 
 2 - Import from https://github.com/confluentinc/cp-helm-charts/tree/master/grafana-dashboard
```
![alt text](https://achong.blob.core.windows.net/gitimages/kafka_overview.PNG)

##### WEB : http://18.144.123.83:3000

### Perform rolling restart of brokers
#### Install and Configure Agent JMX Jolokia 
###### References : https://jolokia.org/download.html

![alt text](https://achong.blob.core.windows.net/gitimages/jolokia_agent.PNG)


#### Run in terminal on each broker instance

* Download and Install JMX jolokia Agent 
```bash
mkdir jolokia
cd jolokia_agent
wget http://search.maven.org/remotecontent?filepath=org/jolokia/jolokia-jvm/1.6.0/jolokia-jvm-1.6.0-agent.jar -O jolokia-agent.jar
```

* Edit kafka service and add line to point to jolokia agent
```bash
sudo nano /etc/systemd/system/broker.service
Environment="EXTRA_ARGS=-javaagent:/home/ubuntu/prometheus/jmx_prometheus_javaagent-0.15.0.jar=8000:/home/ubuntu/prometheus/kafka-2_0_0.yml -javaagent:/home/ubuntu/jolokia_agent/jolokia-agent.jar=host=*"

sudo systemctl daemon-reload
sudo systemctl restart broker

```
* Testing jolokia
```bash
sudo apt install -y jq
curl localhost:8778/jolokia/read/kafka.server:name=UnderReplicatedPartitions,type=ReplicaManager/Value | jq
```
![alt text](https://achong.blob.core.windows.net/gitimages/jolokia.PNG)


#### Run in terminal where the machine you admin your kafka cluster

* Generate public / private key for admin box and after copy them to brokers serverr to enable access
```bash
ssh-keygen

# view content of public key and copy and paste onto brokers instances
cat id_rsa.pub 
```

* 
```bash

```



### 1 - KAFKATOOL - 

### 2 - ZOONAVIGATOR - 

### 3 - KAFKA MANAGER - 

### 4 - KAFKA MONITOR -

### 5 - YAHOO KAFKA MANAGER -

### 6 -  KAFDROP -

