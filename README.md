# freebsd_mailserver

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_mailserver)[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mailserver/) FreeBSD. Install and configure Postfix and Dovecot2.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-mailserver/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


## Requirements and dependencies

### Roles

The roles are not listed in the meta file. Install them manually.

- [vbotka.ansible_lib](https://galaxy.ansible.com/vbotka/ansible_lib) Library of Ansible tasks.

### Collections

- community.crypto
- community.general


## Recommended

- [vbotka.freebsd_mailserver_spamassassin](https://galaxy.ansible.com/vbotka/freebsd_mailserver_spamassassin/)
- [vbotka.freebsd-mailserver_sieve](https://galaxy.ansible.com/vbotka/freebsd_mailserver_sieve/)
- [vbotka.freebsd_mailserver_roundcube](https://galaxy.ansible.com/vbotka/freebsd_mailserver_roundcube/)


## Variables

Review the defaults and examples in vars.


## Workflow

1) Change shell to /bin/sh

```
shell> ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install the roles and collections

```
shell> ansible-galaxy role install vbotka.freebsd_mailserver
shell> ansible-galaxy role install vbotka.ansible_lib
shell> ansible-galaxy collection install community.crypto
shell> ansible-galaxy collection install community.general
```

3) Fit variables, e.g. in vars/main.yml

```
shell> editor vbotka.freebsd_mailserver/vars/main.yml
```

3a) Generate OpenSSL Diffie-Hellman parameters

By default the file *dovecot_ssl_dh* is created by the Ansible module *openssl_dhparam*

```
dovecot_ssl_dh_generate: true
dovecot_ssl_dh_cmd_generate: false
```

It is possible to use custom command *dovecot_ssl_dh_cmd* to create *dovecot_ssl_dh*

```
dovecot_ssl_dh_generate: false
dovecot_ssl_dh_cmd_generate: true
dovecot_ssl_dh_cmd: "openssl dhparam -out {{ dovecot_ssl_dh }} {{dovecot_ssl_dh_bits }}"
```

The options *dovecot_ssl_dh_generate* and *dovecot_ssl_dh_cmd_generate* are mutually exclusive. If
both options are *false* the file *dovecot_ssl_dh_path* is used. This file is provided by the role
for testing only. Never use it in production.

The generation of the file with Diffie-Hellman parameters may take a long time. For example 4096 bit
parameters take ~40min with *Intel(R) Core(TM) i5-8200Y CPU @ 1.30GHz*. It's a good idea to generate
the file separately to speedup the configuration.

```
dovecot_ssl_dh_generate: false
dovecot_ssl_dh_cmd_generate: false
dovecot_ssl_dh_path: <path-to-generated-Diffie-Hellman-file>
```


4) Create playbook and inventory

```
shell> cat freebsd-mailserver.yml

- hosts: mailserver
  roles:
    - vbotka.freebsd_mailserver
```

```
shell> cat hosts
[mailserver]
<mailserver-ip-or-fqdn>
[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=yes
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.7
ansible_perl_interpreter=/usr/local/bin/perl
```

5) Install and configure the mailserver

```
shell> ansible-playbook freebsd-mailserver.yml
```

6) Consider to test the mailserver with http://mxtoolbox.com/


## Check mode

Create default configuration files of Dovecot to avoid error missing files

```
shell> ansible-playbook freebsd-mailserver.yml -t dovecot_example_conf
```

Then run the check-mode

```
shell> ansible-playbook freebsd-mailserver.yml --check
```


## References

- [FreeBSD-Postfix-MySQL-SpamAssassin-Maia-Virtual Setup](http://www.purplehat.org/?page_id=4)
- [Setting up a mail server with OpenSMTPD, Dovecot and Rspamd](https://poolp.org/posts/2019-09-14/setting-up-a-mail-server-with-opensmtpd-dovecot-and-rspamd/)
- [FreeBSD handbook: 28.4. Changing the Mail Transfer Agent](https://www.freebsd.org/doc/handbook/mail-changingmta.html)
- [FreeBSD handbook: 28.9. SMTP Authentication](https://www.freebsd.org/doc/handbook/SMTP-Auth.html)
- [Postfix Documentation](http://www.postfix.org/documentation.html)
- [Postfix SMTP relay and access control](http://www.postfix.org/SMTPD_ACCESS_README.html)
- [Postfix SASL Howto](http://www.postfix.org/SASL_README.html)
- [SASL Authentication in the Postfix SMTP/LMTP client](http://www.postfix.org/SASL_README.html#client_sasl_enable)
- [postfix-logwatch - A Postfix log parser and analysis utility](https://www.freebsd.org/cgi/man.cgi?query=postfix-logwatch)
- [Dovecot Wiki](https://wiki2.dovecot.org/)
- [Upgrading Dovecot v2.2 to v2.3](https://wiki2.dovecot.org/Upgrading/2.3)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.link)
