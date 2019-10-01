# Hadoop_install_macos
Full Hadoop installation

STEP 1: First at all, install HomeBrew. You can download it from http://brew.sh or type in terminal:
```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

STEP 2: Install Hadoop

```
$ brew install hadoop
```

Hadoop will be installed at path /usr/local/Cellar/hadoop
You can find it at "Go", "Go to folder" and type this path.


STEP 3: Configure Hadoop:

Edit hadoop-env.sh, the file can be located at /usr/local/Cellar/hadoop/3.1.2/libexec/etc/hadoop/hadoop-env.sh where 3.1.2 is the hadoop version. Change the line

`
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true" to:

export JAVA_HOME="/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home"
(this line is to have "ResourceManager" in the pack and to launch "start-yarn.sh")
`

AND

`
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc=" Edit Core-site.xml, The file can be located at /usr/local/Cellar/hadoop/3.1.2/libexec/etc/hadoop/core-site.xml add below config
`

<property>
<name>hadoop.tmp.dir</name>
<value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
<description>A base for other temporary directories.</description>
</property>
<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>

Edit mapred-site.xml, The file can be located at /usr/local/Cellar/hadoop/3.1.2/libexec/etc/hadoop/mapred-site.xml and by default will be blank add below config

<configuration>
 <property>
  <name>mapred.job.tracker</name>
  <value>localhost:9010</value>
 </property>
</configuration>

Edit hdfs-site.xml, The file can be located at /usr/local/Cellar/hadoop/3.1.2/libexec/etc/hadoop/hdfs-site.xml add

<configuration>
 <property>
  <name>dfs.replication</name>
  <value></value>
 </property>
</configuration>

To simplify life edit a ~/.profile and add the following commands. By default ~/.profile might not exist.

alias hstart=<"/usr/local/Cellar/hadoop/3.1.2/sbin/start-dfs.sh;/usr/local/Cellar/hadoop/3.1.2/sbin/start-yarn.sh">
alias hstop=<"/usr/local/Cellar/hadoop/3.1.2/sbin/stop-yarn.sh;/usr/local/Cellar/hadoop/3.1.2/sbin/stop-dfs.sh">

and source it by:

```
$ source ~/.profile 
```

If it does not work, try by: ```$ source ~/.bash_profile```

Before running Hadoop format HDFS

```
$ hdfs namenode -format
```

If you had an error about the "option used to bypass 32bit issue warning", add this line:
log4j.logger.org.apache.hadoop.util.NativeCodeLoader=ERROR
in log4j.properties


STEP 4: To verify if SSH Localhost is working check for files ~/.ssh/id_rsa and the ~/.ssh/id_rsa.pub files. If they don’t exist generate the keys using below command

```
$ ssh-keygen -t rsa
```

Enable Remote Login: “System Preferences” -> “Sharing”. Check “Remote Login” Authorize SSH Keys: To allow your system to accept login, we have to make it aware of the keys that will be used

```
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

Test login:
```
$ ssh localhost
```
Last login: The Aug 6 21:56:53 2019 from ::1
```
$ exit (ctrl + c)
```


STEP 5: Run Hadoop

```
$ hstart
```

Test it on your browser by: http://localhost:9870  and http://localhost:8088


and stop using

```
$ hstop
```
