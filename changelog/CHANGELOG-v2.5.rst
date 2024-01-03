===========================================
vbotka.freebsd_mailserver 2.5 Release Notes
===========================================

.. contents:: Topics


2.5.0
=====
Ansible 2.15 update


Release Summary
---------------
* Ansible 2.15 update
* Add FreeBSD 14.0


Major Changes
-------------

Minor Changes
-------------

* Update README. Clarify defaults dovecot_ssl_dh_*

Bugfixes
--------

* Fix files.yml. No parameter *warn* in ansible.builtin.shell

Breaking Changes / Porting Guide
--------------------------------

* Add variable freebsd_mailserver_packages_flavor (default:
  [postfix-sasl]).
