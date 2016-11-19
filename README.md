# Ansible role to deploy OSSEC on servers (manager) and clients (agents)

## Motivation 
* [OSSEC](http://ossec.github.io/) is a must-have on a web server and other machines in a DMZ connected to the Internet since it provides a host-based intrusion detection system.
* [Ansible](https://www.ansible.com/)  is a fantastic tool to automate the deployment of software to different machines
* I needed an easy and automatized way to deploy OSSEC to different machines while giving them some default configuration and still having the possibility to adjust the OSSEC configuration.

## Overview
* Ansible role should work for CentOS, Debian, RedHat, Scientific Linux and Ubuntu and is tested with Ansible version 2.1
* OSSEC gets automatically installed on all machines (server / OSSEC term: manager and clients / OSSEC term: agents).
* All clients / OSSEC term: agents obtain automatically a client key from the server and get a default OSSEC configuration
* The configuration for each client can be individually adjusted.

## Steps in a nutshell to make use of Ansible role

1. Define all machines in the hosts file (both manager and clients).
2. Open the ossec.yml file and in the following section
	```
	authorized: [NameOfServer]
	``` 	change `NameOfServer` to the DNS name of your manager machine, e.g. ossecserver.myorganization.net
1. Open the file `roles/ossec/tasks/main.yml` and replace all occurrences of `NameOfServer`with the name of your manager machine as before.
	* Note: If your manager machine is not using eth0 as its default ethernet port, you have to replace eth0 also by the proper interface name.
	* Note: Probably, one could define the manager DNS name once in Ansible, so that one would not have to replace the manager DNS name a few times. If someone knows how to do that I would be glad to get a PR.
1. If you would like to change the OSSEC default config or to add agent-specific rules, checkout the file `roles/ossec/files/agent.conf` and have a look at the [dedicated manual page](http://ossec.github.io/docs/manual/agent/agent-configuration.html) for further information about configuration options.

## Note on additional copyright / licences
* For the download of YUM packages the AtomiCorp repository and AGPL installation script is used as described in the [OSSEC manual](http://ossec.github.io/docs/manual/installation/installation-package.html)
* The [script to install the YUM package sources of AtomiCorp](https://updates.atomicorp.com/installers/atomic) was slightly modified to allow an automatic installation of the sources.
* Furthermore, it was extended, so that the RedHat packages are also getting installed on Scientific Linux.
* The YUM package sources installation script can be found under `roles/ossec/files/ossec-yum-repository`