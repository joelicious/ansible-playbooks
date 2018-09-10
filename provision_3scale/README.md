Provision an openshift cluster on Fedora
----------------------------------------

These playbooks are meant to be provision docker + openshift + middleware
on a fresh install of Fedora.  These playbooks were tested with Fedora 28 Live Workstation,
leveraging a VM or on bare metal.

Obtain the ISO from [Download Fedora](https://getfedora.org/en/workstation/download/) and 
then start via a VM or provision on a USB stick and install on bare metal. Once installed, 
restart the VM/OS and then follow the steps to create an account. Via the GUI, this account 
should have 'sudo' privileges.

After completing the install, then enable sshd so that ansible can access the host.

```
sudo dnf install -y openssh-server
sudo systemctl start sshd.service
sudo systemctl enable sshd.service
```

This playbook can be used on a single node or multiple nodes.  This inventory file
'hosts' defines the nodes in which docker + openshift + middleware should be configured.

        [playground]
        192.168.1.200
        192.168.62.150

The remote user for each node is defined 'group_vars/playground.xml'.

        remote_user: joe

When executing the playbook, a playbook to provision 'docker' on the host is executed.  Upon
completion, 'openshift' is provisioned - which leverages 'docker'. Finally, 'oc cluster up'
is executed on the host. To execute the script:

        ansible-playbook provision-node.yml -i hosts  -k --ask-become-pass

Once done, you can check the results by browsing to https://[host.ip]:8443 which should
present the OpenShift Admin Console.
 
