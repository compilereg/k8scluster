# K8S cluster setup ansible playbook
An ansible playbook to setup a full kubernetes (k8S) cluster without any perior knowledge. The playbook based on  bsder's playbook , 
URL: https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-ubuntu-18-04

#Changes from bsder's playbook:
  * Remove kube* version from playbook
  * Add docker handler with notification to start
  * Add task to enable docker to start at boot
  * Add when clause  to all kubeadmin settings to limit its execution to cluster admin only.
  * Fixing the url for flannel pod
  * Fixing docker daemon to systemd (https://kubernetes.io/docs/setup/production-environment/container-runtimes/)

# Requirements:
  * Internet connection
  * 1 x Ansible ubuntu server, 1 x K8S admin, at minimum 1 K8S worker
  * Fresh ubuntu box installation on each one
  * Downloading capability

# Installation
On ansible controller:
  * sudo apt update 
  * sudo apt -y install python3-pip ansible sshpass ipcalc 
  * sudo pip3 install j2cli
  * mkdir ansible
  * cd ansible
  * git clone https://github.com/compilereg/k8scluster

# Usage
To setup a K8S cluster, 1st you have to set some parameters as hostnames, and IP addresses. 
Note: the K8S node hostname must be k8sadmin, because it is hardcorded in the task "Join the K8s cluster". You welcome to change it/or make it generic.
configure-system is a shell script asks for parameters values, and generate a file under /tmp for inventory file. 
  * cd k8s_cluster
  * chmod +x configure-system
  * ./configure-system
The script will print the inventory file name, copy it in current direcoty with name hosts. For example if the inventory file was /tmp/compiler
  * cp /tmp/compiler hosts
  * ansible-playbook k8scluster.yml
It may takes time depends on your internet connection and number of k8s cluster nodes

# Automatic inventory file generation
You can automate the configure-system script by create a text file contains all needed parameters and execute it 
 * ./configure-sysyem < <text file name>
The text file structure will be
k8sadmin
<K8s Admin IP>
Integer represents how any workers
<Worker node hostname>
<Worker node IP>
<Administrator username>
<Administrator password>
<sudo password>
<NIC name used in the clusteR>
Then, repeate the worker nodehostname, and IP for your desired number of worker nodes
