

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

* Configure the Cloud ID of our Elasticsearch Service deployment
![alt text](https://chongtech.blob.core.windows.net/chongtechgithubimages/elastic_cloudid.PNG)


* Configuring
```bash
curl broker1:8000
curl broker2:8000 
curl broker3:8000 
```
![alt text](https://achong.blob.core.windows.net/gitimages/prometheus.PNG)


#### Run in terminal on each zookeeper instance

* Download and Install JMX Export Agent 
```bash

```

* Edit Zookeeper service and add line to point to java agent
```bash


```

* Testing and checking mettrics
```bash

```




