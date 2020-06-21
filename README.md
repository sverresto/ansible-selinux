ansible-selinux
===============

A role to add custom selinux policies from .te files.

Motivation:
https://people.centos.org/arrfab/Events/Loadays-2014/managing%20selinux%20with%20your%20cfgmgmt%20solution.pdf

Requirements
------------
Centos 7 or 8. 

Ansibles selinux module is dependent on `libselinux-python` and is
installed for Centos 7 and 8.

Role Variables
--------------

* custom_selinux_dir: /etc/selinux_custom  -- directory to store policy files
* policy_files: []                         -- list of policy files to apply, without .te extension 
* enforce_selinux: true                    -- enforce selinux

Example Playbook
----------------

Add .te files to `files/` directory. (This policy allows httpd to
listen to the telnet port. It is probably not a good idea)

```bash
cat >files/test.te<<EOF
module test 1.0;

require {
	type httpd_t;
	type telnetd_port_t;
	class tcp_socket name_bind;
}

#============= httpd_t ==============
allow httpd_t telnetd_port_t:tcp_socket name_bind;
EOF
```

```ansible
- hosts: all
  become: yes
  vars:
    policy_files:
      - test

  roles:
    - ansible-selinux
```

License
-------

GPL2
