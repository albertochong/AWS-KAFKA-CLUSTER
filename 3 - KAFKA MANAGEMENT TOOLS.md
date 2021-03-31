
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


#### Run in terminal on another machine
###### References : https://prometheus.io/
* Download and install prometheus as service
```bash
# testing from this machine if we can get data from agent/exporter installed ont he broker 
curl 54.151.11.145:8000

# create dir
mkdir prometheus
cd prometheus

# Downlaod
wget https://github.com/prometheus/prometheus/releases/download/v2.26.0/prometheus-2.26.0.linux-amd64.tar.gz

# Unpack
tar -xzvf prometheus-2.26.0.linux-amd64.tar.gz

# Edit configuration an add lines
nano prometheus.yml
################################################################################################
scrape_configs:
  - job_name: 'kafka'
    static_configs:
    - targets: 54.151.11.145:9090
###############################################################################################

# Setup Prometheus as service
sudo nano /etc/systemd/system/prometheus.service
###############################################################################################################

[Unit]
Description=Prometheus Server
Documentation=https://prometheus.io/docs/introduction/overview/
After=network-online.target

[Service]
User=ubuntu
ExecStart=/home/ubuntu/prometheus/prometheus-2.26.0.linux-amd64 --config.file=/home/ubuntu/prometheus/prometheus-2.26.0.linux-amd64/prometheus.yml --storage.tsdb.path=/home/ubuntu/prometheus/prometheus-2.26.0.linux-amd64/data

[Install]
WantedBy=multi-user.target

###############################################################################################################
```

* Reload, enable and start the service
```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
```


### Install and Configure Grafana
##### Run in terminal 

* Edit the hosts file and add the following entries 
```bash

```
##### WEB : 



### KAFKATOOL - http://www.kafkatool.com/features.html

### ZOONAVIGATOR - 

### KAFKA MANAGER - 

### KAFKA MONITOR -

### YAHOO KAFKA MANAGER -

### KAFDROP - https://github.com/obsidiandynamics/kafdrop

