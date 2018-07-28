freebsd_mailserver
==================

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mailserver/) FreeBSD. Install and configure Postfix and Dovecot2.


Requirements
------------

No requiremenst.


Variables
---------

TBD. Review the defaults and examples in vars.


Workflow
--------

1) Change shell to /bin/sh.

```
# ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install role.

```
# ansible-galaxy install vbotka.freebsd_mailserver
```

3) Fit variables.

```
# editor vbotka.freebsd_mailserver/vars/main.yml
```

4) Create playbook and inventory.

```
# cat freebsd-mailserver.yml

- hosts: mailserver
  roles:
    - vbotka.freebsd-mailserver
```

```
# cat hosts
[mailserver]
<mailserver-ip-or-fqdn>
[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python2.7
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure the mailserver.

```
# ansible-playbook freebsd-mailserver.yml
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

- [FreeBSD handbook: 28.4. Changing the Mail Transfer Agent](https://www.freebsd.org/doc/handbook/mail-changingmta.html)
- [FreeBSD handbook: 28.9. SMTP Authentication](https://www.freebsd.org/doc/handbook/SMTP-Auth.html)
- [Postfix Documentation](http://www.postfix.org/documentation.html)
- [Postfix SMTP relay and access control](http://www.postfix.org/SMTPD_ACCESS_README.html)
- [Postfix SASL Howto](http://www.postfix.org/SASL_README.html)
- [SASL Authentication in the Postfix SMTP/LMTP client](http://www.postfix.org/SASL_README.html#client_sasl_enable)
- [postfix-logwatch - A Postfix log parser and analysis utility](https://www.freebsd.org/cgi/man.cgi?query=postfix-logwatch)
- [Dovecot Wiki](https://wiki2.dovecot.org/)

- [Upgrading Dovecot v2.2 to v2.3](https://wiki2.dovecot.org/Upgrading/2.3)
