# freebsd_mailserver

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_mailserver)
[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-mailserver.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-mailserver)
[![GitHub tag](https://img.shields.io/github/v/tag/vbotka/ansible-freebsd-mailserver)](https://github.com/vbotka/ansible-freebsd-mailserver/tags)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_mailserver/) FreeBSD. Install and configure Postfix and Dovecot2.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-mailserver/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


## Requirements and dependencies

### Roles

The roles are not listed in the meta file. Install them manually.

* [vbotka.ansible_lib](https://galaxy.ansible.com/vbotka/ansible_lib) Library of Ansible tasks.

### Collections

The below collections should be part of standard Ansible installation. If necessary install them manually.

* community.crypto
* community.general

### Recommended

- [vbotka.freebsd_mailserver_spamassassin](https://galaxy.ansible.com/vbotka/freebsd_mailserver_spamassassin/)
- [vbotka.freebsd-mailserver_sieve](https://galaxy.ansible.com/vbotka/freebsd_mailserver_sieve/)
- [vbotka.freebsd_mailserver_roundcube](https://galaxy.ansible.com/vbotka/freebsd_mailserver_roundcube/)


## Variables

See the defaults and examples in vars.


## Workflow

1) Change shell to /bin/sh if necessary

```bash
shell> ansible mailserver -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```

2) Install roles

```bash
shell> ansible-galaxy role install vbotka.freebsd_mailserver
shell> ansible-galaxy role install vbotka.ansible_lib
```

Optionally, install roles

```bash
shell> ansible-galaxy role install vbotka.freebsd_mailserver_sieve
shell> ansible-galaxy role install vbotka.freebsd_mailserver_spamassassin
```

3) If necessary install collections

```bash
shell> ansible-galaxy collection install community.crypto
shell> ansible-galaxy collection install community.general
```

4) Fit variables to your needs.

5) Generate OpenSSL Diffie-Hellman parameters

By default the file *dovecot_ssl_dh* is created by the Ansible module *openssl_dhparam*

```yaml
dovecot_ssl_dh_generate: true
dovecot_ssl_dh_cmd_generate: false
```

It is possible to use custom command *dovecot_ssl_dh_cmd* to create *dovecot_ssl_dh*

```yaml
dovecot_ssl_dh_generate: false
dovecot_ssl_dh_cmd_generate: true
dovecot_ssl_dh_cmd: "openssl dhparam -out {{ dovecot_ssl_dh }} {{dovecot_ssl_dh_bits }}"
```

The options *dovecot_ssl_dh_generate* (default: true) and
*dovecot_ssl_dh_cmd_generate* (default: false) are mutually
exclusive. If both options are *false* the file *dovecot_ssl_dh_path*
(default: files/dh.pem) is used. This file is provided by the role for
testing only. Never use it in production.

The generation of the file with Diffie-Hellman parameters may take a
long time. For example 4096 bit parameters take ~40min with *Intel(R)
Core(TM) i5-8200Y CPU @ 1.30GHz*. It's a good idea to generate the
file separately to speedup the configuration.

```yaml
dovecot_ssl_dh_generate: false
dovecot_ssl_dh_cmd_generate: false
dovecot_ssl_dh_path: <path-to-generated-Diffie-Hellman-file>
```

6) Create playbook and inventory

```yaml
shell> cat freebsd-mailserver.yml

- hosts: mailserver
  roles:
    - vbotka.freebsd_mailserver
```

```ini
shell> cat hosts
[mailserver]
<mailserver-ip-or-fqdn>
[mailserver:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_become=true
ansible_become_method=sudo
ansible_python_interpreter=/usr/local/bin/python3.9
ansible_perl_interpreter=/usr/local/bin/perl
```

7) Check the syntax

```bash
shell> ansible-playbook freebsd-mailserver.yml --syntax-check
```

8) Install packages

* Install packages from the role vbotka.freebsd_mailserver

```bash
shell> ansible-playbook freebsd-mailserver.yml -t fm-packages -e fm_install=true

```

* If you enable *sieve*

```yaml
freebsd_mailserver_dovecot_protocols: imap pop3 lmtp sieve
```

install packages from the role vbotka.freebsd_mailserver_sieve

```bash
shell> ansible-playbook freebsd-mailserver-sieve.yml -t fm_ds_packages -e fm_ds_install=true
```

* If you enable *spamassassin*

```yaml
postfix_master_cf_service_def: "inet  n       -       -       -       -       smtpd -o content_filter=spamfilter"
```

install packages from the role vbotka.freebsd_mailserver_spamassassin

```bash
shell> ansible-playbook freebsd-mailserver-spamassassin.yml -t fm_sa_packages -e fm_sa_install=true
```

9) Create default configuration for Dovecot

```bash
shell> ansible-playbook freebsd-mailserver.yml -t dovecot_example_conf
```

10) Dry-run and display changes

```bash
shell> ansible-playbook freebsd-mailserver.yml --check --diff
```

11) Install and configure the mailserver

```bash
shell> ansible-playbook freebsd-mailserver.yml
```

12) Consider to test the mailserver in http://mxtoolbox.com/


## Check mode

Create default configuration files of Dovecot to avoid error missing
files

```bash
shell> ansible-playbook freebsd-mailserver.yml -t dovecot_example_conf
```

Then, run the check-mode

```bash
shell> ansible-playbook freebsd-mailserver.yml --check
```


## Upgrade

### Dovecot

* Set *dovecot_conf_example_recreate=true* and rebuild the Dovecot
  configuration

```bash
shell> ansible-playbook freebsd-mailserver.yml -t fm-dovecot -e dovecot_conf_example_recreate=true
```

* Optionally, set the variables *dovecot_confd_blocks* and
  *dovecot_confd_lines* to your needs and bypass the legacy
  configuration. See tasks/dovecot.yml

```bash
shell> ansible-playbook freebsd-mailserver.yml -t fm-dovecot -e dovecot_confd_legacy=false
```


## Ansible lint

Use the configuration file *.ansible-lint.local* when running
*ansible-lint*. Some rules might be disabled and some warnings might be
ignored. See the notes in the configuration file.

```bash
shell> ansible-lint -c .ansible-lint.local
```


## References

- [FreeBSD handbook: 31.5. Changing the Mail Transfer Agent](https://www.freebsd.org/doc/handbook/mail-changingmta.html)
- [FreeBSD handbook: 31.7.4. SMTP Authentication](https://www.freebsd.org/doc/handbook/SMTP-Auth.html)
- [SMTP Email Server With Postfix](https://subatomicsolutions.org/smtp-email-server-with-postfix)
- [FreeBSD-Postfix-MySQL-SpamAssassin-Maia-Virtual Setup](http://www.purplehat.org/?page_id=4)
- [Setting up a mail server with OpenSMTPD, Dovecot and Rspamd](https://poolp.org/posts/2019-09-14/setting-up-a-mail-server-with-opensmtpd-dovecot-and-rspamd/)
- [Postfix Documentation](http://www.postfix.org/documentation.html)
- [Postfix SMTP relay and access control](http://www.postfix.org/SMTPD_ACCESS_README.html)
- [Postfix SASL Howto](http://www.postfix.org/SASL_README.html)
- [SASL Authentication in the Postfix SMTP/LMTP client](http://www.postfix.org/SASL_README.html#client_sasl_enable)
- [postfix-logwatch - A Postfix log parser and analysis utility](https://www.freebsd.org/cgi/man.cgi?query=postfix-logwatch)
- [Dovecot manual](https://doc.dovecot.org/)
- [OpenDKIM + SPF FreeBSD Forum](https://forums.freebsd.org/threads/opendkim-spf.27201/)
- [OpenDKIM Debian Wiki](https://wiki.debian.org/opendkim)
- [OpenDKIM ArchLinux Wiki](https://wiki.archlinux.org/title/OpenDKIM)


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.info)
