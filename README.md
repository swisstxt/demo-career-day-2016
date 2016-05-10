# Career Day Biel/Bienne Demo 2016

## Description

This project is ment to illustrate how we work at SWISS TXT. It shows a basic step by step setup 
of a kubernetes installation (as used for a quick proof of concept) upon our cloud using ansible 
as well as a deployment of a simple web app on the kubernetes cluster itself. It basically implements 
the instructions given at [this guide](http://severalnines.com/blog/installing-kubernetes-cluster-minions-centos7-manage-pods-services).

## Architecture

![](https://raw.githubusercontent.com/swisstxt/demo-career-day-2016/master/doc/01_arch.png)

In our cloud environment we set up the following components:

__Kubernetes Master:__ The master node provides on tho API and management services (`scheduler`, `etcd`) for Kubernetes

__Kubernetes Minions:__ The minion nodes take orders from the master server and run the containers (services) on a local docker installation

__Jump Host:__ The jump host provides management access (ssh) to the cloud network from outside the cloud (eg. my laptop)

__Public IPs:__ Two public IPs are configured on the cloud router. `jump.stxt.demo` allows to access the jump host. `hello.stxt.demo` in 
load balanced and allows us to access our demo application. 

## Technologies used

__[Ansible](https://www.ansible.com/)__ is a tool used to install and configure servers and services.

__[CloudStack](https://cloudstack.apache.org/)__ is a selfhosted cloud solution, similar to public clouds such as AWS et al.

__[Docker](https://www.docker.com/)__ provides a software infrastructure that allow to packago, deploy and run services in a convenient manner.

__[Kubernetes](http://kubernetes.io/)__ is an open-source system for automating deployment, operations, and scaling of containerized applications.

## Step by Step

### Setup the Environment

In the first step we are going to setup the virtual machines/cloud instances and deploys our ssh key. This enables us to access 
our instances via ssh without password. It also configures a public IP to perform static NAT to the jump host. This allows 
us to ssh on the jump host via public IP from outside the cloud.

All of those steps are performed by ansible via API calls to the cloud API and are executed local... No ssh to the 
machines is required at this stage.

```
# execute step:
ansible-playbook playbooks/01_environment.yml
```

The step can be verified via cloud web interface:

![](https://raw.githubusercontent.com/swisstxt/demo-career-day-2016/master/doc/02_cs_instances.png)

### Configure the Jump Host

This step only reads the inventory files and generates a local ssh configuration that allows us to access all machines in the 
cloud via ssh via the jump host. 

```
# execute step:
ansible-playbook playbooks/02_jumphost_sshconfig.yml

# test: 
ssh -F ssh.config master-01.ma-cloud.local

```

### Configure the Kubernetes Master

Next we prepare the master node according to the documentation: 

### Configure the Kubernetes Minions

### Deploy at Example Service

## It does not end here...

There is always room for improvment! The following steps would help to make our setup a bit more handable, versitile and accessable:

* Move product focused tasks and configuration in ansible roles (eg. role `etcd`, role `kubernetes`, role `hello-career-day-biel-2016`, ...)

