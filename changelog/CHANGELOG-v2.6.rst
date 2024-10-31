===========================================
vbotka.freebsd_mailserver 2.6 Release Notes
===========================================

.. contents:: Topics


2.6.2
=====

Release Summary
---------------
Maintenance update.

Major Changes
-------------
* Supported 13.4 and 14.1
* Add variable dovecot_conf_example_recreate (default=false). If true,
  force copy examples to etc/dovecot
* Update tasks/debug.yml and dovecot.yml
* Add tasks/pam.yml
* Add variable fm_pam_conf (default={})
* Add template pam.j2
* Update tasks/debug.yml and main.yml
* Add vars/pam.yml.sample


Minor Changes
-------------
* Tasks formatting improved.


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
