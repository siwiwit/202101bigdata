Windows download dan install http://cygwin.com/setup-x86_64.exe


Pastikan JAVA_HOME sudah set pada etc/hadoop/hadoop-env.sh
```shell
export JAVA_HOME=/app/java/jdk/current
```

### Pseudo-Distributed Operation

Konfigurasi enable ssh localhost Linux
```shell
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys

```

etc/hadoop/hdfs-site.xml
```xml
<configuration>
  <property>
    <name>dfs.namenode.edits.dir</name>
    <value>file:///live/workspaces/hadoop/hadoop-content/hdfs/namenode</value>
  </property>

  <property>
    <name>dfs.datanode.data.dir</name>
    <value>file:///live/workspaces/hadoop/hadoop-content/hdfs/datanode</value>
  </property>
</configuration>

```
etc/hadoop/core-site.xml:
```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/live/workspaces/hadoop/hadoop-content/tmp</value>
    </property>
</configuration>
```




Format HDFS pertama kali
```shell
bin/hdfs namenode -format
bin/hdfs datanode -format
```

Start Hadoop PseudoCluster = Namenode + Datanode
```shell
sbin/start-dfs.sh
```

Cek pada browser
```shell
http://localhost:9870/
```

Cek log Hadoop
```shell
tail -f logs/hadoop-widi-datanode-localhost.localdomain.log
tail -f logs/hadoop-widi-namenode-localhost.localdomain.logshell
```

Coba buat file
```shell
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/widi
```

```shell
bin/hdfs dfs -mkdir input
bin/hdfs dfs -put etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.0.jar grep input output 'dfs[a-z.]+'
bin/hdfs dfs -cat output/*

```

Stop Hadoop PseudoCluster
```shell
sbin/stop-dfs.sh
```

Bigtop

yum install hadoop hadoop-client hadoop-hdfs* ignite-* flume giraph hadoop-yarn* hbase* hive oozie sqoop* tajo tez zookeeper-server ambari*
