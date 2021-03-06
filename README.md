Ansible Role to Install and Configure Openshource Ansible Tower (AWX)

[![Build Status](https://travis-ci.org/riponbanik/ansible-role-awx.svg?branch=master)](https://travis-ci.org/riponbanik/ansible-role-awx)

## Requirements

PIP, Docker and Git are required. Git is installed via repo package. Other can be installed via the following roles -

    geerlingguy.docker
    geerlingguy.pip

When the install is successful, the server can be accessed using http://host_name_or_ip

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

   data_path: /var/lib/pgdocker

Location whether the postgres will keep the data.

    dns_servers: "8.8.8.8,8.8.4.4"

Modiy to specify our own dns server. Needed to resolve hostname from docker container.

    awx_inventory: inventory

To use your own inventory file.
 
    awx_version: master

To install particular vervison modify it to include git branch name, tag etc.

## Dependencies

VM (on-perm or cloud) is needed to install. Tested with the following OS -

   1. Redhat Enterprise Linux 7
   2. CentOS 7
   3. Unbuntu 18.04 (Bionic) 
   4. Ubuntu 16.04 (Xenial)  

geerlingguy.docker requires container-selinux yum package and can be installed via pre_task in the playbook 

- name: install container-selinux dependency for AWS
          yum:
            name: container-selinux
            enablerepo: rhui-REGION-rhel-server-extras
            state: present
          when: ansible_distribution == 'RedHat' and ansible_distribution_major_version == '7'

- name: install container-selinux dependency for non-AWS
          yum:
            name: container-selinux
            enablerepo: rhel-7-server-extras-rpms
            state: present
          when: ansible_distribution == 'RedHat' and ansible_distribution_major_version == '7'

## Example Playbook

    - name: Install AWX
      hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - role:  riponbanik.awx
    
    - name: Install AWX with custom dns
      hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - role:  riponbanik.awx
          dns_servers: '1.1.1.1, 2.2.2.2'

    - name: Install AWX in AWS 
      hosts: all
      pre_tasks:
        - name: install container-selinux dependency
          yum:
            name: container-selinux
            enablerepo: rhui-REGION-rhel-server-extras
            state: present
          when: ansible_distribution == 'RedHat' and ansible_distribution_major_version == '7'
      roles:
        - role: geerlingguy.docker
        - role: geerlingguy.pip
        - role: riponbanik.awx

## Installation

### Install the role from ansible galaxy to ansible default resarch path
```
sudo ansible-galaxy install riponbanik.awx -p /etc/ansible/roles
```

### Run the installation locally on the machine
```
sudo ansible-playbook -c local -i "localhost," playbook.yml
```

## License

MIT / BSD


## Author Information

This role was created in 2018 by [Ripon Banik](https://www.linkedin.com/in/ripon-banik-79956b23/)

