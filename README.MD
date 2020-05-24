# Deploying Hadoop and Spark on Raspberry Pi Cluster

## Introduction
Today, the requirement of becoming a data scientist has been drastically changed from 10 years ago. Data scientist is capable of not only ETLing data and building, validating, and deploying models but also building front ends (such as dashboard) and increasing efficiency of the entire process (such as distributed computing) since data is huge (Big Data). Knowledge of Big Data tools like the Apache Software Foundation's Hadoop and Spark software is desired in the Data Scientist job description.
However, not everyone can have previlige to access those systems without either appropriate hardware setups (servers) or expenditure on clouding computer (AWS). Despite skills is desirable for many DS jobs, it was difficult to get the neccessory experience with clustering computing without a big investiments. Recently, the hardware improvement of the single board computers, such as Raspberry Pi 4, provides an opportunities that can help DS complete their knowledge of clustering computing.

[Andrew](https://dev.to/awwsmm/building-a-raspberry-pi-hadoop-spark-cluster-8b2) is the pioneer to experiment clustering computing on Raspberriy Pi 4 with 8-nodes in great details, followed by [Liang](https://medium.com/analytics-vidhya/build-raspberry-pi-hadoop-spark-cluster-from-scratch-c2fa056138e0) with 3-node cluster. As Data Scientist, I will utilize 4 Respberry Pi 4B to build my personal cluster and train the ML models on it. For details of distributed computing systems, you can find a great details in Andrew's blog.

This a step-by-step instruction on how to deploying the distributed computing system Hadoop and Spark on a n-node Raspberry Pi cluster. It is a great practice that get you familiar with the setups and functions of the distributed computing system. If you are interested in Data Engineering with direct access to cloud computing system such AWS, this setups would a great portable alternatives.

## Basic Setups
To balance between cost and performance, I chose four Raspberry Pi 4B as the basic nodes. The 4-core CPU is running at 1.5Ghz by default. By tweaking the voltage to 2V, I was able to run the CPU at 1.8Ghz. To o test if the CPU is stable and cooling system is efficient during intensive workload. I run XXX. The detailed spec of the cluster is as below:

### Specs and cost of Respberry Pi 4 Cluster:
  * CPU: 4 X 1.8GHz 64-bit 4-core Cortex-A72 (ARM v8) Soc ($55 each)
  * RAM: 4 X 4GB LPDDR4-3200 SDRAM
  * Ethernet: 4 X Gigabit Ethernet (max 1000Gbps)
  * Ports:  4 X 2 X miniHDMI, 4 X 2 X USB 3.0, 4 X 2 X USB 2.0
  * Storage: 4 X 32GB microSD ()
  * Power: Raspberry Pi Foundation PoE HAT ($20 each)
  * PoE Switch: Unifi 8 Port Switch 60W
  * OS: Raspbian Lite (surprisingly free)
  * Cluster case: GeekPi cluster case ($39.99 not recommended)
  * Cable: 4 X 1-feet ethernet cables ($10)

### Install PoE HAT and cluster
There was a test indicating the old version of PoE HAT may overheat by operating for a long time. I recieved the updated version, hopefully the overheating issue has been migitated.
The PoE HAT package provides four short M2.5 stands and associated screws. To install PoE HAT,
  1. Locate the 40-pin GPIO headers and 4-pin PoE headers on the Raspberry Pi and corresponding slots in the PoE HAT;
  2. Use the provided stands and screws to fasten the stands to the PoE HAT;
  3. Align the stands to the holes of Raspberry pi and insert the PoE HAT on top of the raspberry Pi carefully
  4. Screw the whole package to the stands on one level of the cluster case.

The cluster case originally provides pre-drilled holes for mounting the stands for the Respberry Pi. However, the holes can only fit one pi per level, so I used drill additional holes to support two pi per level. This provides expensive ability of the cluster case for additional four Raspberry Pis.

## Networking
### Enable SSH
Since I installed Raspbrian Lite without Desktop interface, I created an empty SSH file and saved it to the boot partition using a Mac and card reader prior the first boot of the Raspberry Pi.

### Config Static IP Addresses

## Connection

## Hadoop

## Spark

## Steps

## Final thoughts

## Reference
