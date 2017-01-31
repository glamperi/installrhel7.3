Install of Openshift 3.4 using the ansible installer and the xip.io DNS Service

This Openshift install uses ansible, and the enterprise release, which is currently 3.4.  To speed up the getting started process, instead of connecting to your DNS Server, We are using the xip.io, if you are not familar with xip.io, you can click on the link to learn how it works.  Basically, you can modify the hostname and DNS Server configuration after the install to use your Enterprise DNS Server. This is an alternative to 'oc cluster' or CDK.  It allows you to get up quickly and perhaps install some heavier features in a larger VM.  Once Ansible is installed in the VM, you can also use ansible to install in one of your public cloud environments using the playbooks at the openshift-ansible git repo in step #6.  The remainder of the Readme is the steps for the install and post install.

1. Install the RHEL 7.3 ISO 
2. subscription-manager register or use the RHEL install UI to register, make sure to set the network to stay alway on.  Otherwise, you will need to set the device to up in the folder '/etc/sysconfig/network-scripts'.
3. subscription-manager repos \
     --enable="rhel-7-server-rpms" \
    --enable="rhel-7-server-extras-rpms" \
    --enable="rhel-7-server-ose-3.4-rpms"
4. yum install ansible git docker wget
5. yum install -y python-cryptography pyOpenSSL.x86_64
6. git clone https://github.com/openshift/openshift-ansible
7. git clone https://github.com/glamperi/installrhel7.3
8. edit the inventory.erb file in the installrhel7.3 folder, change the master so it resolves to some other name for your RHEL Instance.
9. Edit /etc/hosts and add the master node name(master.YOURIPADDR.xip.io) and the YOURIPADDR is your IP addr (ip addr show) of host with the hostname. Do not use the loop back address for this, the purpose is to be routable with the xip.io DNS Server, see xip.io, if you are not familar with how this works. Also, make sure that the hostname and the master in the inventory.erb file are identical. Edit /etc/hostname and change the hostname, if not.
10. ssh-keygen -t rsa and then copy the key using 'ssh-copy-id root@master.YOURIPADDR.xip.io'
11. ssh root@master.YOURIPADDR.xip.io 
12. run the ansible script - ansible-playbook -i  installrhel7.3/inventory.erb ./openshift-ansible/playbooks/byo/config.yml
13. to Uninstall, run -  ansible-playbook -i  installrhel7.3/inventory.erb /home/gary/openshift-ansible/playbooks/adhoc/uninstall.yml, if changing the hostname, it is best to uninstall before, or else the ansible script will not be able to ssh in.  You might have to do systemctl stop atomic-openshift-master or systemctl stop atomic-openshift-node.
14. Post Install step, you will need a user to login as to the terminal or web console: cd /etc/origin/master and run 'htpasswd -b /etc/origin/master/htpasswd developer developer', this creates the same user as the 'oc cluster' uses by default in the setup.
16. For OADM commands use: oc login -u system:admin --config=/etc/origin/master/admin.kubeconfig






