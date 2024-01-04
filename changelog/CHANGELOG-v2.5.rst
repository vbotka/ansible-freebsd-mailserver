===========================================
vbotka.freebsd_mailserver 2.5 Release Notes
===========================================

.. contents:: Topics


2.5.0
=====

Release Summary
---------------
Ansible 2.15 update. Add FreeBSD 14.0

Major Changes
-------------
* Update dkim, dirs

Minor Changes
-------------
* Update README. Clarify defaults dovecot_ssl_dh_*. If protocol
  *sieve* is used install vbotka.freebsd_mailserver_sieve.
* Split defaults/main.yml to files in defaults/main/
* Remove redundant string quotes in defaults
* Debug format properly indented

Bugfixes
--------
* Fix files.yml. No parameter *warn* in ansible.builtin.shell

Breaking Changes / Porting Guide
--------------------------------
* Add variable freebsd_mailserver_packages_flavor (default:
  [postfix-sasl]).
* New format of fm_dkim_domains. A simple list changed to a list of
  dictionaries (keys: name, selector).
