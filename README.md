users role
==========

User creation role that allow flexible user management from a set of hashes.

Role configuration variables
----------------------------

* users_default_shell (default: /bin/bash) - the default shell if none is
  specified for the user.
* users_create_homedirs (default: true) - create home directories for new
  users. Set this to false is you manage home directories separately.

Data variables
--------------

users
users_groups
ssh_keys



Usage-----

Adding users:

```yaml
users_groups:
- name: testgroup

users:
- name: alice
  comment: Alice
  is_admin: true
  password: '$6$.smI.....'
- name: bob
  comment: Bob
  uid: 555
  is_admin: false
  group: testgroup
- name: claire
  state: absent

ssh_keys:
- user: bob
keys:
- 'ssh-rsa  AAAAB3... bob'
```
