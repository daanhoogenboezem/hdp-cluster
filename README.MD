=======
HDP cluster setup
============================
This project contains some scripts for quickly provisioning ambari-ready CentOS 6.5 VMs. We use VirtualBox for running virtual machines, Vagrant to bring them up and tear then down. Ansible is being used to do the initial bootstrapping.

# Getting started
- Run `vagrant up` to spin up three vm's and have them bootstrapped.
- Run `vagrant ssh master` to ssh into the master node.
- Become root and run `ambari-server setup` to start the ambari setup process. When prompted, stick with the defaults.
- Run `ambari-server start` to start the web ui.

# Ambari configuration
- Point your browser to `http://localhost:8080` and login with credentials `admin/admin`.
- Click on `Launch install wizard`, provide a cluster name and hit next.
- Select `HDP 2.3`, enter `master.locallab.com`, `slave1.locallab.com`, `slave2.locallab.com` in the dropdown. Paste the generated private key from `provisioning/roles/master-keys/files/id_rsa` in the `Host Registration Information` textarea and hit `Register and confirm`
- During the `Confirm hosts` step, confirm that all hosts checks succeeded and click next.
- In the `Choose services` step tick the `HDFS`, `YARN + MapReduce2`, `Zookeeper` and `Spark` checkboxes and hit next.
- Make `master.locallab.com` the master node for all services, hit next.
- Make `slave1.locallab.com` and `slave2.locallab.com` slaves for all services, hit next.
- Accept suggestions during the `Customize services` and `Review` steps. Wait for Ambari to complete.
