# Career Day Biel/Bienne Demo 2016

## Description

This project is ment to illustrate how we work at SWISS TXT. It shows a basic step by step setup of a kubernetes installation (as used for a quick proof of concept) upon our cloud using ansible as well as a deployment of a simple web app on the kubernetes cluster itself. It basically implements the instructions given at [this guide](http://severalnines.com/blog/installing-kubernetes-cluster-minions-centos7-manage-pods-services).

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

[![](https://raw.githubusercontent.com/swisstxt/demo-career-day-2016/master/doc/02_cs_instances.png)

### Configure the Jump Host

### Configure the Kubernetes Master

### Configure the Kubernetes Minions

### Deploy at Example Service

## It does not end here...

There is always room for improvment! The following steps would help to make our setup a bit more handable, versitile and accessable:

* Move product focused tasks and configuration in ansible roles (eg. role `etcd`, role `kubernetes`, role `hello-career-day-biel-2016`, ...)

