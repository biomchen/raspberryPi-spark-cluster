# Deploying Hadoop and Spark on Raspberry Pi Cluster

## Introduction
Today, the requirement of becoming a data scientist has been drastically changed from 10 years ago. Data scientist is capable of not only ETLing data and building, validating, and deploying models but also building front ends (such as dashboard) and increasing efficiency of the entire process (such as distributed computing) since data is huge (Big Data). Knowledge of Big Data tools like the Apache Software Foundation's Hadoop and Spark software is desired in the Data Scientist job description.
However, not everyone can have previlige to access those systems without either appropriate hardware setups (servers) or expenditure on clouding computer (AWS). Despite skills is desirable for many DS jobs, it was difficult to get the neccessory experience with clustering computing without a big investiments. Recently, the hardware improvement of the single board computers, such as Raspberry Pi 4, provides an opportunities that can help DS complete their knowledge of clustering computing.

[Andrew](https://dev.to/awwsmm/building-a-raspberry-pi-hadoop-spark-cluster-8b2) is the pioneer to experiment clustering computing on Raspberriy Pi 4 with 8-nodes in great details, followed by [Liang](https://medium.com/analytics-vidhya/build-raspberry-pi-hadoop-spark-cluster-from-scratch-c2fa056138e0) with 3-node cluster. As Data Scientist, I will utilize 4 Respberry Pi 4B to build my personal cluster and train the ML models on it.

This a step-by-step instruction on how to deploying the distributed computing system Hadoop and Spark on a n-node Raspberry Pi cluster. It is a great practice that get you familiar with the setups and functions of the distributed computing system. If you are interested in Data Engineering with direct access to cloud computing system such AWS, this setups would a great portable alternatives.

## Basic Setups
To balance between cost and performance, I chose four Raspberry Pi 4B as the basic nodes. The 4-core CPU is running at 1.5Ghz by default. By tweaking the voltage to 2V, I was able to run the CPU at 1.8Ghz. To o test if the CPU is stable and cooling system is efficient during intensive workload. I run

### Specs and cost of Respberry Pi 4 Cluster:
  * 1.8GHz 4-core CPU with 4GB memory ($57 each)
  * 32 GB Samsung EVO microSD ($7 each)
  * PoE HAT ($20 each)
  * Unifi 8 Port Switch 60W ($112)
  * Raspbian Lite OS (surprisingly free)
  * GeekPi cluster case ($39.99 not recommended)
  * 4 1-feet ethernet cables ($10)

## Security

## Connection

## Hadoop

## Spark

## Steps

## Final thoughts

## Reference
