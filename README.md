Install of Openshift 3.4 using the ansible installer

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
9. Edit /etc/hosts and add the master node name(master.myhost.io) and the IP addr (ip addr show) of host with the hostname. It does not seem to work with the loopback IP address. Also, make sure that the hostname and the master in the inventory.erb file are identical.
10. ssh-keygen -t rsa
11. ssh root@master.myhost.io 
12. run the ansible script - ansible-playbook -i  installrhel7.3/inventory.erb ./openshift-ansible/playbooks/byo/config.yml
13. to Uninstall, run -  ansible-playbook -i  installrhel7.3/inventory.erb /home/gary/openshift-ansible/playbooks/adhoc/uninstall.yml 
14. cd /etc/origin/master
15. htpasswd -b /etc/origin/master/htpasswd developer developer
16. oc login -u system:admin --config=/etc/origin/master/admin.kubeconfig






