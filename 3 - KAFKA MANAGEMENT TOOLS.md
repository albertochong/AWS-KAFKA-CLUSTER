
# Kafka Administration and Monitoring TOOLS
Here you can find some tools for cluster kafka management and monitoring.
By defautl Kafka give mettrics exposing JMX(Java Management)

### Install and Configure Agent JMX and Prometheus Server 
###### References : https://github.com/prometheus/jmx_exporter
######              https://www.confluent.io/blog/monitor-kafka-clusters-with-prometheus-grafana-and-confluent/?mkt_tok=NTgyLVFIWC0yNjIAAAF8Hi4wNGl0q-            t3VaEYdXbWJ3R6HmPiTBhuCYUnAC8_UZtyx7bRV6p44p0LpoGYwIFIhQ2eHl3__PucX08sPLY4tU-jHVI4WZKuwiD4q-uAHVlO
######              https://medium.com/@alvarobacelar/monitorando-um-cluster-kafka-com-ferramentas-open-source-a4032836dc79
                    
#### Run in terminal on each broker instance

* Downlaod and Install JMX Export Agent 
```bash
mkdir prometheus
cd prometheus
wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.15.0/jmx_prometheus_javaagent-0.15.0.jar
wget https://raw.githubusercontent.com/prometheus/jmx_exporter/master/example_configs/kafka-2_0_0.yml
```

* Edit kafka service and add line to point to java agent
```bash
sudo nano /etc/systemd/system/broker.service
Environment="EXTRA_ARGS=-javaagent:/home/ubuntu/prometheus/jmx_prometheus_javaagent-0.15.0.jar=8000:/home/ubuntu/prometheus/kafka-2_0_0.yml"

sudo systemctl daemon-reload
sudo systemctl restart broker

```

* Testing and checking mettrics
```bash
curl broker1:8000 
```
![alt text](https://achong.blob.core.windows.net/gitimages/prometheus.PNG)


#### Run in terminal on another machine to install and configure Prometheus Server
###### References : https://prometheus.io/

* Edit the hosts file and add the following entries 
```bash
sudo nano /etc/hosts
172.31.22.104 zookeeper1
172.31.31.206 zookeeper2
172.31.22.176 zookeeper3

172.31.3.111  broker1
172.31.5.198  broker2
172.31.10.181 broker3
```

* Download and install prometheus Server as service
```bash
# testing from this machine if we can get data from agent/exporter installed ont he broker 
curl broker1:8000
curl broker2:8000
curl broker3:8000

# Downlaod
wget https://github.com/prometheus/prometheus/releases/download/v2.12.0/prometheus-2.12.0.linux-amd64.tar.gz

# Unpack
sudo tar -zxvf prometheus-2.12.0.linux-amd64.tar.gz /opt/

# Edit configuration an add lines
sudo /opt/prometheus-2.12.0.linux-amd64/prometheus.yml
################################################################################################


scrape_configs:
  - job_name: 'kafka'
    static_configs:
    - targets: ['172.31.3.111:8000', '172.31.5.198:8000', '172.31.10.181:8000'] #brokers


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

* Add prometheus as datasource in grafana
![alt text](https://achong.blob.core.windows.net/gitimages/grafana_add_datasource.PNG)



### Install and Configure Grafana
#### Run in terminal on another machine to install and configure Grafana
###### References : https://grafana.com/


* Download and install grafna Server as service
```bash

# Downlaod
wget https://dl.grafana.com/oss/release/grafana-7.5.2.linux-amd64.tar.gz

# Unpack
sudo tar -zxvf grafana-7.5.2.linux-amd64.tar.gz /opt/

# Edit configuration an add lines
sudo /opt/grafana-7.5.2/conf/defaults.ini
################################################################################################





###############################################################################################

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

##### WEB : http://18.144.123.83:3000



### KAFKATOOL - 

### ZOONAVIGATOR - 

### KAFKA MANAGER - 

### KAFKA MONITOR -

### YAHOO KAFKA MANAGER -

### KAFDROP - https://github.com/obsidiandynamics/kafdrop

