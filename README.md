freebsd-mailserver
==================

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver)

[Ansible role](https://galaxy.ansible.com/vbotka/ansible-freebsd-mailserver/). Install and configure postfix and dovecot2 with FreeBSD.

Tested with FreeBSD 10.3 at [digitalocean.com](https://cloud.digitalocean.com)


Requirements
------------

No requiremenst.


Variables
---------

TBD (Check the defaults).


Workflow
--------

1) Change shell to /bin/sh.

```
ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
ansible-galaxy install vbotka.ansible-freebsd-mailserver
```

3) Fit variables.

```
~/.ansible/roles/vbotka.ansible-freebsd-mailserver/vars/main.yml
```

4) Create playbook and inventory.

```
> cat ~/.ansible/playbooks/freebsd-mailserver.yml
---
- hosts: mailserver
  become: yes
  become_method: sudo
  roles:
    - role: vbotka.ansible-freebsd-mailserver
```

```
> cat ~/.ansible/hosts
[mailserver]
<MAILSERVER-IP-OR-FQDN>

[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python2
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure the mailserver.

```
ansible-playbook ~/.ansible/playbooks/freebsd-mailserver.yml
```

6) Consider to test the mailserver with http://mxtoolbox.com/


License
-------

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


Author Information
------------------

[Vladimir Botka](https://botka.link)

References
----------
- [Postfix SMTP relay and access control](http://www.postfix.org/SMTPD_ACCESS_README.html)
