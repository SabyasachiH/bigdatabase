Creating Hadoop Clusters
=======================
This document explains how to create Hadoop clusters with Spark, Scala, and Hive on top of PostgreSQL. 

You can do the following implement types: 

- **Vagrant With Oracle VirtualBox**: Single-node implementation.

- **Vagrant AWS**: Single node implementation with orchestration done by Vagrant, provisioning by Ansible.

- **Bare Metal**: By creating your own inventory file on any machine or node. 

You will get the following components when you deploy software using Vagrant:

|Software | Version|
| ------ | ----- | 
|Linux box ||
|Vagrant AWS plugin | 
|Hadoop| 2.7.4|
|Hive| 1.2.2|
|Spark| 2.1.1|
|Scala| 2.11.6|
|PostgreSQL| 9.5 (for hive metastore)|
|Anaconda2| 5.0.1|
|Node| 8.9.0|

# Requirements
-------------

Here the list of software along with their versions. 

|Software | Version|Download Link|Comments|
| ------ | ----- | ----- | ----- | 
|Vagrant |2.1.1|[Click here.](http://www.vagrantup.com/downloads.html)|Required for Oracle VirtualBox and AWS deployments.|
|Oracle VirtualBox |5.0.18|[Click here.](https://www.virtualbox.org/wiki/Downloads)|Required for Oracle VirtualBox deployments.|
|Ansible |2.4.1.0 |[Click here.](https://releases.ansible.com/ansible/)|Required for all deployment types.|
|SSHPASS||[Click here.](https://gist.github.com/arunoda/7790979)|Required for all deployment types.|


Deploying on Oracle VirtualBox
------------------------------
Follow these steps to deploy clustering using OVB:

1. Install Oracle VirtualBox 5.0.18, Vagrant 2.1.1, Ansible 2.4.1.0, and SSHPASS. 
2. Download and extract the latest ShareInsights source from here: [Releases](https://github.com/datacell/bigdatabase/releases).
3. In your terminal change your directory into the project directory
(i.e. `cd bigdatabase`)
4. Execute the command ```vagrant up``` to create the VM
5. Execute ```vagrant ssh``` to login to the VM

Deploying on AWS 
-----------------
Follow these steps to deploy clustering on AWS:

1. Install Vagrant 2.1.1, Ansible 2.4.1.0, and SSHPASS. 
2. Download and extract the latest ShareInsights source from here: [Releases](https://github.com/datacell/bigdatabase/releases).
3. In your terminal change your directory into the project directory
(i.e. `cd bigdatabase`)
4. Rename the Vagrantfile-aws to Vagrantfile
5. Execute the command ```vagrant up``` to create the VM
6. Execute ```vagrant ssh cedric-1.0.0-1``` to login to the VM


Deploying on Bare-Metal Devices 
-------------------------------

Follow these steps to deploy clustering on bare-metal devices:
1. Install Ansible 2.4.1.0 and SSHPASS. 
2. In your terminal change your directory into the project directory
(i.e. `cd bigdatabase`)
3. Run the ansible_wrapper.sh ```./ansible_wrapper.sh -b <Target machine hostname> <Target machine ip> <Target machine ssh port> <Target machine ssh user> [hadoop]```
4. If Java is to be installed, please set setup_java to True in the vars file

Note: 
1. Hadoop will not be installed if parameter is not specified
2. Script will ask for sudo passwords multiple times during execution, please ensure provided user has elevated rights

# Web-user Interfaces
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
* YARN resource manager:  (http://publicip:8088)
* HDFS: (http://publicip:50070/dfshealth.html)
* Spark history server: (http://publicip:18080)
* Spark context UI (if a Spark context is running): (http://publicip:4040)
[Spark context server port is open from 4040 to 4044]

# Shared Folder (VirtualBox Only)
--------------------------------------------------------------------------------
Vagrant automatically mounts the folder containing the Vagrant file from the
host machine to the guest machine as `/vagrant` inside the guest.

# Management of Vagrant VM
--------------------------------------------------------------------------------
To stop the VM, while preserving the setup and data within the VM: 

```
vagrant halt
```

or

```
vagrant suspend
```

Execute `vagrant up` command again to restart the VM from where you left off.


To completely **wipe** the VM so that `vagrant up` command gives you a fresh
machine: 

```
vagrant destroy
```

After that, execute: `vagrant up` command as usual.



# Starting / Stoping Services Manually
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

Thanks to Andrew Rothstein for the great work at:
(https://github.com/andrewrothstein/ansible-anaconda)

Thanks to Martin Robson and Xiaomei-data for great work at:
(https://github.com/martinprobson/vagrant-hadoop-hive-spark)
(https://github.com/xiaomei-data/ansible-hadoop-spark)
