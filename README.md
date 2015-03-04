[![Build Status](https://travis-ci.org/willshersystems/ansible-users.svg?branch=master)](https://travis-ci.org/willshersystems/ansible-users)
[![Ansible Galaxy](http://img.shields.io/badge/galaxy-willshersystems.users-660198.svg?style=flat)](https://galaxy.ansible.com/list#/roles/2562)

User management
===============

Configure users, group, SSH keys and administrative sudo access. The role can:

* Add and remove groups
* Add and remove users
* Install SSH keys for users
* Configure sudo to allow root access for the administration group

Requirements
------------

Tested on:

* Ubuntu 14.04 LTS

Should work with:

* All Ubuntu
* All Debian
* All EL 6,7 based distros

Role Variables
--------------

### Users

* users

A list containing dictionaries of configuration. Each list item can have the
following elements. *Italic text* indicates the default.

  * name **required**
  * state (*present*)
  * uid
  * group (*item.name*)
  * groups
  * shell (*{{ users_default_shell }}*)
  * comment (*Unknown user*)
  * password (*{{ users_default_password }}*)
  * home (*/home/{{item.name}}*)
  * createhome (*{{ users_create_homedirs }}*)
  * system
  * is_admin

*users_default_password* is set to watching the default password is for new
accounts for the given OS, e.g. !!

All other variable defaults are given below.

*is_admin* is a boolean. If set, the user is added to the OSs admin group.

All other items correspond to the ansible users module parameters.

#### Example

```yaml
users:
- name: alice
  comment: Alice
  password: '$6$hE1TkH.X$F9flUhXGiL8zI4lOioqp1FxN8L8HrMbuP4ZFVbv5MnyeQFAhIAsbTmT6t7.p93FgyiJo3U/aJeiGHzCTUvA.s.'
  is_admin: yes
  groups:
    - devs
    - mgmt
- name: bob
  state: absent
```

### Groups

* users_groups

A list of user groups to create. Each list item can have the following elements.
*Italic text* indicates the default.

  * name **required**
  * state (*present*)
  * system
  * gid

#### Example
  
```yaml
users_groups:
- name: devs
  gid: 1000
- name: mgmt
  gid: 2000
- name: hr
  state: absent
```

### SSH Keys

* users_ssh_keys

A list of users, each containing a key with a list of public SSH keys as its
value.

  * name
  * keys

name is the username.

keys is a list of public SSH keys.

#### Example

```yaml
users_ssh_keys:
- name: alice
  keys:
    - ssh-rsa AAAA..... alice@somewhere.com
    - ssh-rsa AAAA..... alice@elsewhere.com
- name: bob
  keys:
    - ssh-rsa AAAA.... bob@somewhere.com
```

### Tuning configuration

* users_default_shell

The default shell for a user if none is specified. Defaults to `/bin/bash`

* users_create_homedirs

Create home dirs for new users? Set this to false if you manage home directories
in some other way. Defaults to *true*.

* users_per_user_groups

Create groups for users with the same name as the users group. Defaults to
*true*

### Sudo configuration

* users_manage_admin_sudoers

If true, create sudo configuration to allow users in the admin group to become
root via sudo. Defaults to *true*

* users_admin_uses_ansible

If true, assume the admin group is also used to run Ansible jobs. This disables
requiretty for the admin group in the sudoers configuration. Defaults to *true*

* users_admin_sudo_password

If true require a password for sudo, false to not require a password. Defaults
to *true*

Dependencies
------------

None

Example Playbook
----------------

```yaml
---
- hosts: all
  sudo: true
  vars:
    users_admin_sudo_password: false
  roles:
  - role: willshersystems.users
    users:
    - name: carl
      uid: 1010
      comment: 'Carl Crisp'
      is_admin: yes
      groups:
      - beancounters
    - name: ec_user
      state: absent
    users_groups:
    - name: beancounters
    users_ssh_keys:
    - name: carl
      keys:
      - ssh-rsa AAAA....... carl@accounts.example.com
```

License
-------

LGPLv3

Author Information
------------------

Matt Willsher, matt@willsher.systems

(c)2015 Willsher Systems
