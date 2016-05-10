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

__Public IPs:__ Two public IPs are configured on the cloud router. `jump.stxt.demo` allows to access the jump host. `hello.stxt.demo` is 
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
us to ssh to the jump host via public IP from outside the cloud.

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
ssh -F ssh.config root@master-01.ma-cloud.local

```

### Configure the Kubernetes Master

Next we prepare the master node according to the documentation: 

* `etcd` and `kubernetes` will be installed
* `etcd` will be configured
* the kubernetes API server will be configured
* all required services will be started
* the virtual network for docker will be configured

```
# execute step:
ansible-playbook playbooks/03_k8s-master.yml

# test: 
ssh -F ssh.config root@master-01.ma-cloud.local "kubectl get nodes"
```

### Configure the Kubernetes Minions

Similar to the last steps, the minion nodes will be prepared: 

* `flannel` and `kubernetes` will be installed
* `flannel` will be configured
* `kubelet` sevices will be configured
* all required services will be started

```
# execute step:
ansible-playbook playbooks/04_k8s-minion.yml

# test: 
ssh -F ssh.config root@master-01.ma-cloud.local "kubectl get nodes"
```

### Deploy at Example Service

In this step, a kubernetes pod (a single container or a group of containers) as well its service (a presentation of the 
container to the outside world of kubernetes) will be deployed via master to the minions.

```
# execute step:
ansible-playbook playbooks/05_k8s-service.yml

# test: 
ssh -F ssh.config root@master-01.ma-cloud.local "kubectl get rc"
ssh -F ssh.config root@master-01.ma-cloud.local "kubectl get services"
```

### Configure the load balancer

In this step, a kubernetes pod (a single container or a group of containers) as well its service (a presentation of the 
container to the outside world of kubernetes) will be deployed via master to the minions.

```
# execute step:
ansible-playbook playbooks/05_k8s-service.yml
```

To test just point a browser at http://hello.stxt.demo (host entry needs to be configured)

## Achievements

We now have... 

* A working kubernetes environment
* An automated and reproducable installation
* Documentation for free

## It does not end here...

There is always room for improvment! The following steps would help to make our setup a bit more handable, versitile and accessable:

* Move product focused tasks and configuration in ansible roles (eg. role `etcd`, role `kubernetes`, role `hello-career-day-biel-2016`, ...)
* Make the master services (`etcd`, `apiservice`, ...) redundant
* The service can be deployed in much nicer ways (look at http://docs.ansible.com/ansible/kubernetes_module.html for example)
* ... 

