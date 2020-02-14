
# Confluent Kafka Cluster Installation and Configuration in Multi Broker 

## Run in terminal 

* Download Kafka under home/use directory (with hdoop user in Namenode)
```bash
sudo wget 
```

* unpack files
```bash
sudo tar -xzf hadoop-3.2.1.tar.gz
```

* Move to folder opt/hadoop and change owner
```bash
sudo mv hadoop-3.2.1/ /opt/hadoop
sudo chown -R hadoop:hadoop /opt/hadoop
``` 

* Testing installation
```bash
cd /opt/hadoop/bin/./hadoop version
```


* Add some envirnoment variables under home/user
```bash
nano .bash_profile
# HADOOP
     export HADOOP_HOME=/opt/hadoop
     export HADOOP_INSTALL=$HADOOP_HOME
     export HADOOP_COMMON_HOME=$HADOOP_HOME
     export HADOOP_MAPRED_HOME=$HADOOP_HOME
     export HADOOP_HDFS_HOME=$HADOOP_HOME
     export YARN_HOME=$HADOOP_HOME
     export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
``` 

*  renitialize to activate new variables
```bash
source .bash_profile
``` 

*  Check hadoop version if installed sucessfully
```bash
hadoop version
``` 

## NameNode Configuration
   (With hadoop user connect)

*  Edit file nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh and add this lines:
```bash
export JAVA_HOME=/opt/jdk
export HADOOP_HOME=/opt/hadoop
export HADOOP_CONF_DIR="${HADOOP_HOME}/etc/hadoop"
export PATH="${PATH}:${HADOOP_HOME}/bin"
``` 

*  Edit file nano $HADOOP_HOME/etc/hadoop/core-site.xml  and add this lines:
```bash
<configuration>
   <property>
      <!-- HDFS PATH -->
     <name>fs.default.name</name>
     <value>hdfs://ip-172-31-8-80.us-east-2.compute.internal:19000</value><!--- dns namenode -->
   </property>
</configuration>
``` 

*  Create some directories
```bash
mkdir /opt/hadoop/dfs
mkdir /opt/hadoop/dfs/data
mkdir /opt/hadoop/dfs/namespace_logs
``` 


*  Edit file nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml and add this lines: 
```bash
<configuration>
    <property>
      <name>dfs.replication</name>
      <value>3</value>
    </property>
    <property>
      <name>dfs.namenode.name.dir</name>
      <value>/opt/hadoop/dfs/namespace_logs</value>
    </property>
    <property>
      <name>dfs.datanode.data.dir</name>
      <value>/opt/hadoop/dfs/data</value>
    </property>
    <property>
      <name>dfs.namenode.checkpoint.dir</name>
      <value>/opt/hadoop/dfs/namesecondary</value>
    </property>
</configuration>
``` 

*  Edit file nano $HADOOP_HOME/etc/hadoop/workers and add datanodes dns: 
```bash
ip-172-31-2-100.us-east-2.compute.internal
ip-172-31-14-249.us-east-2.compute.internal
``` 

*  Edit file nano $HADOOP_HOME/etc/hadoop/mapred-site.xml and add this lines: 
```bash
<configuration>
    <property>
       <name>mapreduce.job.user.name</name>
       <value>hadoop</value>
    </property>
   
   <property>
      <name>yarn.resourcemanager.address</name>
      <value>ip-172-31-8-80.us-east-2.compute.internal:8032</value><!-- dns namenode. -->
   </property>

   <property> 
	    <name>mapreduce.framework.name</name> 
	    <value>yarn</value> 
   </property>

   <property>
     <name>yarn.app.mapreduce.am.env</name>
     <value>HADOOP_MAPRED_HOME=/opt/hadoop</value>
   </property>

   <property>
     <name>mapreduce.map.env</name>
     <value>HADOOP_MAPRED_HOME=/opt/hadoop</value>
   </property>

   <property>
     <name>mapreduce.reduce.env</name>
     <value>HADOOP_MAPRED_HOME=/opt/hadoop</value>
   </property>

</configuration>
``` 

*  Edit file nano $HADOOP_HOME/etc/hadoop/yarn-site.xml and add this lines: 
```bash
<configuration>

  <property>
     <name>yarn.resourcemanager.hostname</name>
     <value>ip-172-31-8-80.us-east-2.compute.internal</value><!-- dns namenode  -->
  </property>

  <property>
     <name>yarn.nodemanager.resource.memory-mb</name>
     <value>1536</value>
  </property>

  <property>
     <name>yarn.scheduler.maximum-allocation-mb</name>
     <value>1536</value>
  </property>

  <property>
     <name>yarn.scheduler.minimum-allocation-mb</name>
     <value>128</value>
  </property>

  <property>
     <name>yarn.nodemanager.vmem-check-enabled</name>
     <value>false</value>
  </property>

  <property>
     <name>yarn.server.resourcemanager.application.expiry.interval</name>
     <value>60000</value>
   </property>

  <property>
     <name>yarn.nodemanager.aux-services</name>
     <value>mapreduce_shuffle</value>
   </property>

  <property>
     <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
     <value>org.apache.hadoop.mapred.ShuffleHandler</value>
   </property>

  <property>
     <name>yarn.log-aggregation-enable</name>
     <value>true</value>
   </property>

  <property>
     <name>yarn.log-aggregation.retain-seconds</name>
     <value>-1</value>
   </property>

  <property>
     <name>yarn.application.classpath</name>
   <value>$HADOOP_CONF_DIR,${HADOOP_COMMON_HOME}/share/hadoop/common/*,${HADOOP_COMMON_HOME}/share/hadoop/common/lib/*,${HADOOP_HDFS_HOME}/share/hadoop/hdfs/*,${HADOOP_HDFS_HOME}/share/hadoop/hdfs/lib/*,${HADOOP_MAPRED_HOME}/share/hadoop/mapreduce/*,${HADOOP_MAPRED_HOME}/share/hadoop/mapreduce/lib/*,${HADOOP_YARN_HOME}/share/hadoop/yarn/*,${HADOOP_YARN_HOME}/share/hadoop/yarn/lib/*</value>
   </property>
   
</configuration>
``` 

## DataNode Configuration

* On each Datanodes wuth hadoop user
```bash
sudo mkdir /opt/hadoop
cd /opt
sudo chown -R hadoop:hadoop /opt/hadoop/
``` 

* Go to NameNode and connect with hadoop user and copy folder hadoop to all DataNodes.<br>
  Another option is create one image from NameNode and copy to DataNodes
```bash
cd ~
scp -rv  /opt/hadoop  hadoop@ip-172-31-2-100.us-east-2.compute.internal:/opt ------------datanode1
scp -rv  /opt/hadoop  hadoop@ip-172-31-14-249.us-east-2.compute.internal:/opt------------datanode2
``` 

## Starting cluster    
* with hadoop user on Node Master.
```bash
hdfs namenode -format
```  

* Start HDFS.
```bash
$HADOOP_HOME/sbin/start-dfs.sh
``` 

* Start YARN.
```bash
$HADOOP_HOME/sbin/start-yarn.sh
``` 

* Checking services.
```bash
jps
``` 

* Check Report.
```bash
hdfs dfsadmin -report
```   
 
 NOTE: 
       1 - if you doesn't change hadoop parameter cnfiguration it´s will assume default configuration and save on temporary folder tmp
           Hadoop Oficial doc : https://hadoop.apache.org/docs/stable/index.html
           
       2 - Check hadoop logs in opt/hadoop/logs if any error come up. If you want to remove some logs files in terminal run
           rm -rf * ------> run inside logs folder
           
       3 - you can format namenode again or drop all content inside folder tmp/hadoop-xxxxxx if any problem occurs but not 
           in production environment

       4 - Check my namenode on : http://13.59.94.15:9870/
           Check my Datanode1 on : http://3.20.97.41:9864
           Check my Damenode2 on : http://3.20.36.83:9864
