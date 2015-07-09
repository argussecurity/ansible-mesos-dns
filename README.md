ansible-mesos-dns
=========

Ansible role for installing and configuring mesos-dns in the cluster. Can choose either `server` or `client` setup.

Mesos-dns is deployed on Marathon, assuming it is run on Mesos master machine.

Based on https://github.com/mesosphere/mesos-dns/blob/master/contrib/ansible-gce/roles

Requirements
------------

None.

Role Variables
--------------

- `mesos_dns_install_mode` should be `server` or `client`
- `mesos_masters` list of Mesos master hosts, used in `server` setup
- `marathon_port` defaults to 8080
- `dns_host` used in `client` setup to configure the dns server address

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: [mesos_workers[0]]
      sudo: True
      roles:
        - { role: 'ansible-mesos-dns-server', mesos_masters: "{{ groups.mesos_primaries }}", tags: ['dns-server'] }
    - hosts: [mesos_workers]
      sudo: True
      roles:
        - { role: 'ansible-mesos-dns-client', dns_host: "{{ groups.mesos_workers[0] }}", tags: ['dns-client'] }
