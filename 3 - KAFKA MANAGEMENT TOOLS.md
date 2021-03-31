
# Kafka Administration and Monitoring TOOLS
Here you can find some tools for cluster kafka management and monitoring.
By defautl Kafka give mettrics exposing JMX(Java Management)

### Install and Configure Prometheus 
###### References : https://github.com/prometheus/jmx_exporter
######              https://www.confluent.io/blog/monitor-kafka-clusters-with-prometheus-grafana-and-confluent/?mkt_tok=NTgyLVFIWC0yNjIAAAF8Hi4wNGl0q-            t3VaEYdXbWJ3R6HmPiTBhuCYUnAC8_UZtyx7bRV6p44p0LpoGYwIFIhQ2eHl3__PucX08sPLY4tU-jHVI4WZKuwiD4q-uAHVlO
                    
#### Run in terminal on each broker instance

* Downlaod and Install JMX Export Agent 
```bash
mkdir prometheus
cd prometheus
wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.15.0/jmx_prometheus_javaagent-0.15.0.jar
wget https://raw.githubusercontent.com/prometheus/jmx_exporter/master/example_configs/kafka-2_0_0.yml
```

* Edit kafka service and add line to point to prometheus
```bash
sudo nano /etc/systemd/system/broker.service
Environment="EXTRA_ARGS=-javaagent:/home/ubuntu/prometheus/jmx_prometheus_javaagent-0.15.0.jar=8000:/home/ubuntu/prometheus/kafka-2_0_0.yml"

sudo systemctl daemon-reload
sudo systemctl restart broker

```

* Testing and checking
```bash
curl broker1:8000 
```
![alt text](https://achong.blob.core.windows.net/gitimages/prometheus.PNG)

### Install and Configure Grafana
##### Run in terminal 

* Edit the hosts file and add the following entries 
```bash
sudo nano /etc/hosts
172.31.22.104 zookeeper1
172.31.31.206 zookeeper2
172.31.22.176 zookeeper3
```
##### WEB : 



### KAFKATOOL - http://www.kafkatool.com/features.html

### ZOONAVIGATOR - 

### KAFKA MANAGER - 

### KAFKA MONITOR -

### YAHOO KAFKA MANAGER -

### KAFDROP - https://github.com/obsidiandynamics/kafdrop

