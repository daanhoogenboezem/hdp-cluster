=======
HDP cluster setup
============================
This project contains some scripts for quickly provisioning ambari-ready CentOS 6.5 VMs. We use VirtualBox for running virtual machines, Vagrant to bring them up and tear then down. Ansible is being used to do the initial bootstrapping.

#Prerequisites
Prior to working with this project, the following components should be installed on your machine :

 * [Packer (0.8.6)](https://www.packer.io/downloads.html)
 * [VirtualBox (5.0.10)](https://www.virtualbox.org/wiki/Downloads)
 * [Vagrant (1.7.4)](https://www.vagrantup.com/downloads)
 * [Ansible (1.9.4)](http://www.ansible.com/home)

# Building CentOS 6.5 basebox
- Step into directory `centos-6.5`
- Inspect and validate the CentOS 6.5 template by running `packer inspect template.json` and `packer validate template.json`
- Build the basebox by running `packer build -force -only=virtualbox-iso -var autologin=true template.json`. At the `Unsupported Hardware Detected` dialog, hit enter once.
- Publish the resulting basebox to artifactory using the credentials that were provided to you, optionally updating the `box_url` paramaters in the `Vagrantfile`

# Getting started
- Run `vagrant up` to spin up three vm's and have them bootstrapped.
- Run `vagrant ssh master` to ssh into the master node.
- Become root and run `ambari-server setup` to start the ambari setup process. When prompted, stick with the defaults.
- Run `ambari-server start` to start the web ui.

# Ambari configuration
- Point your browser to `http://192.168.100.10:8080` and login with credentials `admin/admin`.
- Click on `Launch install wizard`, provide a cluster name and hit next.
- Select `HDP 2.3`, enter `master.locallab.com`, `slave1.locallab.com`, `slave2.locallab.com` in the dropdown. Paste the generated private key from `provisioning/roles/master-keys/files/id_rsa` in the `Host Registration Information` textarea and hit `Register and confirm`
- During the `Confirm hosts` step, confirm that all hosts checks succeeded and click next.
- In the `Choose services` step tick the `HDFS`, `YARN + MapReduce2`, `Zookeeper` and `Spark` checkboxes and hit next.
- Make `master.locallab.com` the master node for all services, hit next.
- Make `slave1.locallab.com` and `slave2.locallab.com` slaves for all services, hit next.
- Accept suggestions during the `Customize services` and `Review` steps. Wait for Ambari to complete.

# Verify setup
- Step into directory `/usr/hdp/current/spark-client`
- As the hdfs user, execute the command `./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn-cluster --num-executors 3 --driver-memory 512m --executor-memory 512m --executor-cores 1 lib/spark-examples*.jar 10`
