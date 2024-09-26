===========================================
vbotka.freebsd_mailserver 2.6 Release Notes
===========================================

.. contents:: Topics


2.6.1
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------

Minor Changes
-------------
- Update python 3.11 in .travis.yml
- Update tests/test.yml playbook

Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------


2.6.0
=====

Release Summary
---------------
Ansible 2.17 update.

Major Changes
-------------
* Add supported FreeBSD 14.1

Minor Changes
-------------
* Update README, lint config
* Update debug
* Add var fm_role_version
* Add var al_supported_versions_override_rel
* Update handlers listen to lowercase names
* Fix lint
* Set ownership and permissions of DKIM private keys. Skip check mode
  in fm_dkim_create_keys
  
Bugfixes
--------

Breaking Changes / Porting Guide
--------------------------------
