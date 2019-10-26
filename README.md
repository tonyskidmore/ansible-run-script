# ansible-run-script
A simple demo ansible role to run a script.

There are occasions when you may have a bash script that you would like to test in an automated fashion.  You might not have Ansible installed and available for the purpose and configured in your environment, you might have a pre-existing script that does everything you want or you might have a different delivery method for deploying and executing scripts e.g. [Azure Custom Script Extension](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/custom-script-linux).  

Whatever the reason we can still use [Ansible](https://www.ansible.com/) to help with development and testing.  We can use [Molecule](https://github.com/ansible/molecule) to deploy a script into a variety of Linux distributions and verify that the script works as expected.  I had a search on GitHub for a suitable candidate script and selected [otseca](https://github.com/trimstray/otseca) as an example use case, it looked particularly good as it is aimed at verifying the state of a Linux system from a security perspective.  

Here we be testing with a number of Linux distibutions and we will use Ansible and Molecule to create the test platforms based on Docker containers, deploy the script and verify that it works as expected.  Spoiler alert, it fails on some of the test platforms, which is a great example to highlight the testing process and gives the opportunity to develop the script further to work in these failing environments.

Refer to the [Molecule Installation](https://molecule.readthedocs.io/en/stable/installation.html) to get the necessary pre-requisites installed on your test environment.  You will also need Docker installed.  I am using CentOS 7.7 for my testing so you can refer to [Get Docker CE for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/) if you use that platform too or refer to [Install Docker](https://docs.docker.com/install/) for information for other systems.  

After you have installed the above then you can test things for yourself by carrying out the following steps:

```
git clone https://github.com/tonyskidmore/ansible-run-script.git
cd ansible-run-script
molecule test
```
To bring up the testing environment (without destroying at the end which `molecule test` does), execute the role but leave the containers running:
```
molecule converge
```
To run just the verify stage (currently just checks the script has been deployed and whether report files have been received):
```
molecule verify
```
To connect to one of the hosts and check why the script may not have created the report successfully:
```
molecule login --host ubuntu1804
otseca --ignore-failed --tasks system,network --output /tmp/report
```
To destroy the running instances after you have finished testing:
```
molecule destroy
```

For an excellent article on using Molecule visit [Testing your Ansible roles with Molecule](https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule) by [Jeff Geerling](https://www.jeffgeerling.com/)
