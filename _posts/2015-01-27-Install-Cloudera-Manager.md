---
title: Install Cloudera Manager on AWS EC2
layout: post
---
Cloudera Manager and the managed CDH services are easy to install and run. I configured on AWS EC2: one instance for Cloudera Manager Server, two other instances for the CDH Cluster. The Cloudera Manager Server manages the CDH Cluster through the Cloudera Agent installed on the cluster's two instances. 

# CDH Services Run on the Cluster
I only installed the most basic services on the cluster: ZooKeeper, HDFS and YARN. The topology on the 2-instance cluster: one acts as the server, hosting ZooKeeper, HDFS NameNode, YARN ResourceManager and JobHistoryManager; the other as worker, hosting HDFS DataNode and YARN NodeManager. All instances are accessible from internet through an Elastic IP.

# Several Notes
1. Before launching instances on AWS EC2, create as many new Elastic IPs and Network Interfaces. Associate the Elastic IPs to the Network Interfaces. This would maximally allow the instances' Public IPs to be used in Cloudera Manager web UI, e.g. for job/log monitoring. Associate the Network Interfaces to the new instances when launching them in EC2.

2. Right after we launch the instances, update the OSes to the latest, then edit`/etc/sysctl.conf` and put one line `vm.swappiness = 1`. (Cloudera Manager 5.3.1 installer still suggests using 0 for `vm.swappiness` however [this Cloudera blog post][1] suggests 1.)

3. Also install `ntp` package on all instances to run the ntp service. Reboot all instances. Check the `vm.swappiness` value by `cat /proc/sys/vm/swappiness`.

4. Download the Cloudera Manger install file from the website. There're 2 ways of installation: web UI or manual. If we follow the manual installation, just do the Cloudera Manager Server bit. Once the Cloudera Manager Server is up, visit its web link `http://<server>:7180` and deploy the Cloudera Agent and CDH pakages to the cluster instances through the web UI.

5. Once the Cloudera Agent and CDH parcels are deployed to the cluster instances. We'll be asked to choose which CDH Services to add to the cluster for running. I'd recommend choosing first ZooKeeper, HDFS and YARN as a basic start.

6. I had it once that the Cloudera Manager Server EBS volume was down and I had to re-setup everything. So machine fails in real life! For the CDH Cluster, HDFS and YARN could be configured for High Availability. The vulnerable point here seems to be the Cloudera Manager Server in the entire architecture.

[1]: http://blog.cloudera.com/blog/2015/01/how-to-deploy-apache-hadoop-clusters-like-a-boss/
