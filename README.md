# HDP
Ansible scripts to install and configure Hortonworks Data Platform on Vagrant.

## Requirements

Install [Vagrant](https://www.vagrantup.com/), [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [VirtualBox Oracle Extensions](https://www.virtualbox.org/wiki/Downloads) and the `vagrant-hostmanager` and `vagrant-vbguest` plugins:

```bash
vagrant plugin install vagrant-hostmanager
vagrant plugin install vagrant-vbguest
```

[Ansible](https://www.ansible.com/) is another requirement.  While it can be [installed globally](http://docs.ansible.com/ansible/intro_installation.html), installing the Ansible source, or using a [Python virtual environment](http://docs.python-guide.org/en/latest/dev/virtualenvs/) is rather helpful:

```bash
# Ansible source method
git clone git://github.com/ansible/ansible.git --recursive
source ./ansible/hacking/env-setup -q

# Virtual environment method
pip install virtualenv
virtualenv ansible
source ansible/bin/activate
pip install ansible

# prompt should have (ansible) prefix, e.g.
# (ansible) [08:45 AM]{mra9161:R5060116(master)}:HDP

# deactivate to go back to regular python
deactivate
```

# Vagrant

Some useful Vagrant commands

- `vagrant up` -- start the VMs described in `Vagrantfile`
- `vagrant provision` -- run the `hdp.yml` [ansible playbook](http://docs.ansible.com/ansible/playbooks.html) against the VMs
- `vagrant reload` -- needed when settings change, will not provision
- `vagrant ssh <machine>` -- ssh as the `vagrant` user
- `vagrant suspend` -- suspend the VMs
- `vagrant destroy` -- destroy the VMs, add `-f` to force

