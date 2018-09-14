Creating Hadoop Clusters
=======================
This document explains how to create Hadoop clusters using Spark, Scala, and Hive on top of PostgreSQL, and Python version Anaconda2 5.0.1.

You can implement the following clustering architectures: 

- **Vagrant With Oracle VirtualBox**: Single-node implementation.

- **Vagrant AWS**: Single node or three node implementation with orchestration done by Vagrant, provisioning by Ansible.

- **Bare Metal**: By creating your own inventory file on any machine or node. 

# Requirments
-------------

Here the list of software along with their versions. 

|Software | Version|
| ------ | ----- | 
|Vagrant |2.1.1|
|Oracle VirtualBox |5.0.18|
|Vagrant AWS plugin | |
|Ansible |2.4.1.0 |
|Linux box ||
|Hadoop| 2.7.4|
|Hive| 1.2.2|
|Spark| 2.1.1|
|Scala| 2.11.6|
|PostgreSQL| 9.5 (for hive metastore)|
|Anaconda2| 5.0.1|
|Node| 8.9.0|


Vagrant project to spin up a virtual machine running cluster on top of
64 bit Ubuntu/Xenial:

* Hadoop 2.7.4
* Hive 1.2.2
* Spark 2.1.1
* Scala 2.11.6
* Postgresql 9.5 (for hive metastore)
* Anaconda2 5.0.1
* Node 8.9.0


The virtual machine will be running the following services:

* HDFS NameNode + DataNode
* YARN ResourceManager/NodeManager + JobHistoryServer + ProxyServer
* Hive metastore and server2
* Spark history server

Deploying on Oracle VirtualBox
------------------------------
Follow these steps to deploy clustering using OVB:

1. Download and install Oracle VirtualBox 5.0.18 using the instructions given [here.](https://www.virtualbox.org/wiki/Downloads)
2. Download and install Vagrant 2.1.1 using the instructions given [here.](http://www.vagrantup.com/downloads.html)
3. Download and install Ansible 2.4.1.0 using the instructions given [here.](https://releases.ansible.com/ansible/)
4. Download and install SSHPASS using the instructions given [here.](https://gist.github.com/arunoda/7790979)
5. Download and extract the latest ShareInsights source from here: [Releases](https://github.com/datacell/bigdatabase/releases).
6. In your terminal change your directory into the project directory
(i.e. `cd bigdatabase`)
7. Execute the comamnd ```vagrant up``` to create the VM
8. Execute ```vagrant ssh``` to login to the VM

Deploying on AWS 
-----------------
Follow these steps to deploy clustering on AWS:

1. Download and install Vagrant 2.1.1 using the instructions given [here.](http://www.vagrantup.com/downloads.html)
2. Download and install Ansible 2.4.1.0 using the instructions given [here.](https://releases.ansible.com/ansible/)
3. Download and install SSHPASS using the instructions given [here.](https://gist.github.com/arunoda/7790979)
4. Download and extract the latest ShareInsights source from here: [Releases](https://github.com/datacell/bigdatabase/releases).
5. In your terminal change your directory into the project directory
(i.e. `cd bigdatabase`)
6. Rename the Vagrantfile-aws to Vagrantfile
7. Execute the command ```vagrant up``` to create the VM
8. Execute ```vagrant ssh cedric-1.0.0-1``` to login to the VM


Deploying on Bare-Metal Devices 
-------------------------------

Follow these steps to deploy clustering on bare-metal devices:
1. Download and install Ansible 2.4.1.0 using the instructions given [here.](https://releases.ansible.com/ansible/)
2. Download and install SSHPASS using the instructions given [here.](https://gist.github.com/arunoda/7790979)
3. Download and extract the latest ShareInsights source from here: [Releases](https://github.com/datacell/bigdatabase/releases).
4. In your terminal change your directory into the project directory
(i.e. `cd bigdatabase`)
5. Run the ansible_wrapper.sh ```./ansible_wrapper.sh -b <Target machine hostname> <Target machine ip> <Target machine ssh port> <Target machine ssh user> [hadoop]```
6. Note: Hadoop will not be installed if parameter is not specified
7. Note: Script will ask for sudo passwords multiple times during execution, please ensure provided user has elevated rights
8. If Java is to be installed, please set setup_java to True in the vars file

# Web user interfaces
--------------------------------------------------------------------------------
Here are some useful links to navigate to various UI's:
--------------------------------------------------------------------------------
VirtualBox
--------------------------------------------------------------------------------

* YARN resource manager:  (http://localhost:8088)
* HDFS: (http://localhost:50070/dfshealth.html)
* Spark history server: (http://localhost:18080)
* Spark context UI (if a Spark context is running): (http://localhost:4040)
[Spark context server port is open from 4040 to 4044]

--------------------------------------------------------------------------------
AWS
--------------------------------------------------------------------------------
* YARN resource manager:  (http://<publicip>:8088)
* HDFS: (http://<publicip>:50070/dfshealth.html)
* Spark history server: (http://<publicip>:18080)
* Spark context UI (if a Spark context is running): (http://<publicip>:4040)
[Spark context server port is open from 4040 to 4044]

# Shared Folder (VirtualBox Only)
--------------------------------------------------------------------------------
Vagrant automatically mounts the folder containing the Vagrant file from the
host machine into the guest machine as `/vagrant` inside the guest.


# Managment of Vagrant VM
--------------------------------------------------------------------------------
To stop the VM and preserve all setup/data within the VM: -

```
vagrant halt
```

or

```
vagrant suspend
```

Issue a `vagrant up` command again to restart the VM from where you left off.


To completely **wipe** the VM so that `vagrant up` command gives you a fresh
machine: -

```
vagrant destroy
```

Then issue `vagrant up` command as usual.



# Starting / stoping services manually
--------------------------------------------------------------------------------

```
$ vagrant ssh
$ sudo su - hadoop

# Stop the services
$ jps | grep -v Jps | awk '{print $1}' | xargs kill -9

# Start the services
$ /bin/bash /opt/service-start-cluster.sh
```


# Credits

Thanks to Andrew Rothstein for the great work at
(https://github.com/andrewrothstein/ansible-anaconda)

Thanks to Martin Robson and xiaomei-data for great work at
(https://github.com/martinprobson/vagrant-hadoop-hive-spark)
(https://github.com/xiaomei-data/ansible-hadoop-spark)
