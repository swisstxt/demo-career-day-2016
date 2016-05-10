# Career Day Biel/Bienne Demo 2016

## Description

This project is ment to illustrate how we work at SWISS TXT. It shows a basic step by step setup 
of a kubernetes installation (as used for a quick proof of concept) upon our cloud using ansible 
as well as a deployment of a simple web app on the kubernetes cluster itself. It basically implements 
the instructions given at [this guide](http://severalnines.com/blog/installing-kubernetes-cluster-minions-centos7-manage-pods-services).

## Architecture

## Technologies used

### Ansible

### CloudStack

### Docker

### Kubernetes

## Step by Step

### Setup the Environment

```
# execute step:
ansible-playbook playbooks/01_environment.yml
```

In the first step we are going to setup the virtual machines/cloud instances. It also configures a public IP 
to perform static NAT to the jump host. This allows us to ssh on the jump host via public IP from outside the cloud.

All of those steps are performed by ansible via API calls to the cloud API and are executed local... No ssh to the 
machines is required at this stage.

The step can be verified via cloud web interface:

![](https://raw.githubusercontent.com/swisstxt/demo-career-day-2016/master/doc/02_cs_instances.png)

### Configure the Jump Host

### Configure the Kubernetes Master

### Configure the Kubernetes Minions

### Deploy at Example Service

## It does not end here...

There is always room for improvment! The following steps would help to make our setup a bit more handable, versitile and accessable:

* Move product focused tasks and configuration in ansible roles (eg. role `etcd`, role `kubernetes`, role `hello-career-day-biel-2016`, ...)

