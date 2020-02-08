---
layout: post
title: "OpenShift 4.2 Cluster on a Single Node"
description: "Follow these steps to deploy OpenShift 4.2 cluster on a single node. This deploys 3 masters and 3 computes."
date:   2020-02-08 17:46:41 -0300
categories: openshift-installation
by: 'Tosin Akinosho'
icon: 'credit-card'
---
# Installing OpenShift 4 Using Qubinode

This guide should get you up and running with qubinode.

# Introduction

The qubinode is a prescriptive installation of either Red Hat OpenShift Platform or The Origin Community Distribution of Kubernetes.


# Installing Red Hat Enterprise Linux

* *[RHEL Installation Walkthrough](https://developers.redhat.com/products/rhel/hello-world#fndtn-rhel)* - Follow baremetal steps
* Get your developer subscription of RHEL
* Download the RHEL 7.7 iso
* From the software selection choose: Virtualization Host > Virtualization Platform
* If using two storage devices install choose the correct one for RHEL installation. If using the recommend storage options. Install RHEL on the ssd. The installer will delicate the majority of your storage to /home, you can choose "I will configure partitioning" to have control over this.
* Configure NETWORK & HOSTNAME
* Begin installation
* set root password and create admin user with sudo privilege


# Prepare the system

One RHEL has been installed on your system, ensure the system has internet connection. The qubinode-installer expects that your already have DHCP running in your environment for the purpose of supplying IP addresses for the VM nodes.

**Login remotely to the Qubinode box as  as the qubinode admin user**

```
ssh <your-user>@<your-system-ip-address>
```

**Download the qubinode-installer project**
```
wget https://github.com/Qubinode/qubinode-installer/archive/2.3.zip
unzip 2.3.zip
mv qubinode-installer-2.3 qubinode-installer
rm -f 2.3.zip
cd qubinode-installer/
```

**Download the qcow image**
*From your web browser:*
* Navigate to: https://access.redhat.com/downloads/content/69/ver#/rhel---7/7.7/x86_64/product-software
* Find *Red Hat Enterprise Linux 7.7 Update KVM Guest Image (20191016)* and right click on the *Download Now" box
* wget -c "insert-url-here" -O rhel-server-7.7-update-2-x86_64-kvm.qcow2


## Quick start
```
./qubinode-installer
```
> Select Option 1

**Option 1  will perform the following**
* Configure server for KVM.
* Deploy an idm server to be used as DNS.
* Deploy OpenShift 4.
* Optional: Configure NFS Provisioner

# Advanced installation

## Use this when you would like to step thru the installation process.
**setup playbooks vars and user sudoers**  
```
./qubinode-installer -m setup
```

**register host system to Red Hat***  
```
./qubinode-installer -m rhsm
```
**Update to RHEL 7.7 if you are using 7.6**
```
sudo yum update -y
```

**setup host system as an ansible controller**
```
./qubinode-installer -m ansible
```

**setup host system as an ansible controller**
```
./qubinode-installer -m host
```

**Download qcow images for idmserver**
```
copy rhel-server-7.7-update-2-x86_64-kvm.qcow2 to qubinode-installer directory
```

**install idm dns server**
```
./qubinode-installer -p idm
```

**Optional: Uninstall idm dns server. This may be used when there is an issue with the deployment. Note that idm is required for OpenShift 4.x installations.**
```
./qubinode-installer  -p idm -d
```

**Prerequisites**
```
Please download your pull-secret from:
https://cloud.redhat.com/openshift/install/metal/user-provisioned
and save it as /home/admin/qubinode-installer/pull-secret.txt
```

**Install OpenShift 4.x**
```
./qubinode-installer -p ocp4
```

**Optional: Uninstall idm dns server**
```
./qubinode-installer  -p ocp4 -d
```

# Deployment Post Steps

## How to access OpenShift Cluster
* Option 1: Add dns server to /etc/resolv.conf on your computer.
  - Or run script found under lib/qubinode_dns_configurator.sh
* Option 2: Add dns server to router so all machines can access the OpenShift Cluster.
