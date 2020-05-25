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
  * OS: Raspbian 2020-02-13 (surprisingly free)
  * JAVA: openJDK 1.8.0_212, JRE build 8u212-b01-1+rpi1-b01, Client VM build 25.212-b01, mixed mode.
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
After you SSHing your raspberry pi, don't forget to change the password using `passwd` command if your raspberry pis are in your home network.

### Config Static IP Addresses and Hostnames
The reason to use the Unifi Switch instead of the other cheaper options is that the Unifi Switch can be integrated to my home network easily. I've already had a Unifi Controller, Security Gateway, and 8 port Switch. By adding additional switch, I can access the cluster easily from any where in my home.

I enabled static IP address for each node and router's IP address to route the internet traffic in my home network by invoking:
```bash
$ sudo nano /etc/dhcpcd.conf
```
to uncomment the lines in the file:

```bash
interface eth0
static ip_address=192.168.1.20/24
static routers=192.168.1.1
```
The 192.168.1.20 is the IP address of the master node; 192.168.1.21 – 192.168.1.23 are of worker nodes. So you have to repeated edits in each node.

```bash
$ sudo nano /etc/hosts
```

```bash
192.168.1.20    Spark-Master   
192.168.1.21    Spark-Worker-1
192.168.1.22    Spark-Worker-2
192.168.1.23    Spark-Worker-3
```
```bash
sudo nano /etc/hostname
```

```bash
Spark-Master or Spark-Work-X
```
where X indicates the node (1,2, or 3 for my cluster).

After you have set hostnames for all nodes, you can alias the hostnames to ssh command using

```bash
$ sudo nano ~/.ssh/config
```
```bash
Host Spark-Master
User pi
Hostname 192.168.1.20

Host Spark-Worker-1
User pi
Hostname 192.168.1.21

Host Spark-Worker-2
User pi
Hostname 192.168.1.22

Host Spark-Worker-3
User pi
Hostname 192.168.1.23
```
If you don't have `~/.ssh/` in the home directory, you should create one before you can add and edit the `config` file:

```bash
$ mkdir .ssh
$ nano ~/.ssh/config
```
Now you can use alias to SSH each node of the cluster. Don't forget to add this your config file if you want to alias to SSH each node.

### SSH with public/private keys pairs
```bash
$ ssh-keygen -t ed25519
```
No passphrase is necessary to protect the access of key pair.

#### **The Steps from Config Static IP to SSH with keys pair can be done repeated from first pi to last pi.**

Concatenate each public key to the Master node; perform the command on the rest nodes:
```bash
$ cat ~/.ssh/id_ed25519.pub | ssh pi@Spark-Master 'cat >> .ssh/authorized_keys'
```
After public keys of other pis copied to authorized_keys file of the Master node, you need copy the Master node's public key to the authorized_keys file as well.
Then you use `scp` to securely copy the authorized_keys file to the worker nodes
```bash
$ scp ~/.ssh/authorized_keys Spark-Worker-1:~/.ssh/authorized_keys
$ scp ~/.ssh/authorized_keys Spark-Worker-2:~/.ssh/authorized_keys
$ scp ~/.ssh/authorized_keys Spark-Worker-3:~/.ssh/authorized_keys
```
Don't forget to copy `~/.ssh/config` file.
```bash
$ scp ~/.ssh/config Spark-Worker-1:~/.ssh/config
$ scp ~/.ssh/config Spark-Worker-2:~/.ssh/config
$ scp ~/.ssh/config Spark-Worker-3:~/.ssh/config
```

### Adding functions to .bashrc
Functions can ease your tasks since you have multiple nodes in your cluster. Alternatively, there is package to utilize the clusters. Here I follow Andrew's suggestions to implement the functions in the .bashrc file:

```bash
# get hostname of all the nodes
function otherpis {
  grep "pi" /etc/hosts | awk '{print $2}' | grep -v $(hostname)
}

# commands on all nodes; run the commands on other nodes first, then run it on the current node
function clustercmd {
  for pi in $(otherpis); do ssh $pi "$@"; done
  $@
}

# securely copy to all nodes
function clusterscp {
  for pi in $(otherpis); do
    cat $1 | ssh $pi "sudo tee $1" > /dev/null 2>&1
  done
}

# reboot all nodes
function clusterreboot {
  clustercmd sudo shutdown -r now
}

# shutdown all nodes
function clustershutdown {
  clustercmd sudo shutdown now
}
```
After editing .bashrc, don't forget to
```bash
$ source ~/.bashrc
```
and copy all .bashrc to other nodes
```bash
$ clusterscp ~/.bashrc
```
### Firewalls and Security
You can install `ufw` and edit rules to protect the cluster from unauthorized access. Or you can use `fail2ban` to monitor the ssh service after you have modified rules for login. See [Andrew's post](https://dev.to/awwsmm/building-a-raspberry-pi-hadoop-spark-cluster-8b2) for details.

## Hadoop
### Single node



## Spark

## Steps

## Final thoughts

## Reference
