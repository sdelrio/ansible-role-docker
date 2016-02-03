[![Build Status](https://travis-ci.org/sdelrio/ansible-role-docker.svg)](https://travis-ci.org/sdelrio/ansible-role-docker)

Docker
======

Install Docker from dockerproject.org repository.

Requirements
------------

Debian, the repository has jessie, streetch, wheezy.
Ubuntu, the repository has precise, trusty, utopic, vivid and wily.

Requires pip installed on the system. You can use the role `mbasanta.pip` from ansible galaxy as you can see on the example below, since I had problems with pip from Debian jessie repository.

Only tested with jessie and kernels 3.16, 4.2 & 4.3.

Role Variables
--------------

- `docker_users`: Users to add to docker group. Example: if you are using a vagrant machine, you could add vagrant user to docker group so you can use docker without sudo or becoming root.

Dependencies
------------

No dependencies.

Example Playbook
----------------

```yaml
---
- name: Deploy Docker services
  hosts: all
  become_user: root
  pre_tasks:
    - name: update the apt cache
      apt: update_cache=yes cache_valid_time=28800
      when: ansible_os_family == 'Debian'
    - name: check if pip exist
      command: pip -V
      register: pip_command
      changed_when: "pip_command.rc != 0"
  roles:
    - role: mbasanta.pip
      vars:
        - pip_install_method: "installer"
        - pip_install_url: "https://bootstrap.pypa.io/get-pip.py"  }
      when: pip_command.rc != 0
      tags: docker
    - role: ansible-role-docker
      vars:
        - docker_users:
            - vagrant
```

License
-------

BSD

