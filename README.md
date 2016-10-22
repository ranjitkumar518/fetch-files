fetch-files
=========

Ansible Role: fetch-file
------------

Ansible role that fetches to get your specified files on target machines.

Requirements
------------

* Ansible version 2.0 < 

Role Variables
--------------

Available variables are listed below,

defaults/main.yml:
```
savedir: "fetched"
```

vars/main.yml:

```
lists:
  - { filename: '/etc/ssh/sshd_config' }
  - { filename: '/var/log/syslog' }
  - { filename: '/etc/nsswitch.conf' }
  - { filename: '/etc/SuSE-release' }
```

This roles was created with automatic when you run a playbook.
You only create a text file listed to get files below,

Ex) sample_filelist.txt:
```
/etc/ssh/sshd_config
/usr/bin/python
/etc/nsswitch.conf
```

Dependencies
------------

None

Example Playbook
----------------

```
- name: Playbook for fetching files
  hosts: "{{ target }}"
  gather_facts: no

  become: true
  become_user: root

  vars_prompt:
    - { name: "target" , prompt: "Input target host" ,  default: all , private: no }
    - { name: "inputfile" , prompt: "Input your file list" , default: sample_filelist.txt , private: no }
    - { name: "savedir" , prompt: "Input a save directory" , default: fetched , private: no }

  pre_tasks:
    - block:
        - name: Pre setup (create lists for fetch)
          local_action: raw ./tools/create_var1.pl "{{ inputfile }}"
      become: false

  roles:
    - fetch_file

```

License
-------

BSD

Author Information
------------------

fetch-file role was written by: T.Kuramochi
