 role ssh_authorized_keys
==============================
Ansible Rolle to manage and deploy ssh keys of admin and non-admin users

 combinations
---------------
It is highly recomended to use this role together with a role to manage users and to manage the sshd configuration.<br/>
The following roles are tested in combination and work well - at least for the user [DO1JLR](https://github.com/do1jlr):
 - [github.com/chaos-bodensee/role-manage_users](https://github.com/chaos-bodensee/role-manage_users.git)
 - [github.com/chaos-bodensee/role-ssh_authorized_keys](https://github.com/chaos-bodensee/role-ssh_authorized_keys.git) *(this one)*
 - [github.com/chaos-bodensee/role_sshd](https://github.com/chaos-bodensee/role_sshd.git)

```txt
Protipp:

Deploy the manage_users role *before* deploying the ssh keys.
If the user does not exist it is hard to add a ssh key for him!
```

 Variables
---------

* ``admins`` (default ``[]``):<br/>
  A list of ``ssh`` keys allowed to log in as `root`.

* ``accounts`` (default ``[]``):<br/>
  A list of usernames that will be created on this host, if they don't exisit

* `users` (default `{}`):<br/>
  A dict of user names mapping to lists of ``ssh`` keys
  allowed to log in to the given user account.

* ``ssh_public_key_store`` (default ``ssh_public_keys``):<br/>
  A directory path where the public key files can be found by ansible.

For aditional variables please have a look into ``defaults/main.yml``!

To add extra SSH Keys from github to a user use the ``github_users: {}`` settings

 Files
-----

This role assumes that the *public* parts of all required ``ssh`` keys
can be found within the directory ``ssh_public_key_store``. The file
names must follow the convention: ``username_idalg.pub`` are are matched
by the ``username`` part.


 Examples
--------

Alice and Bob may log in and are allowed to become ``root`` with the ``sudo`` command on this host:

```
admins:
  - alice
  - bob
```

Alice, Bob and Eve may log in to ther own user accounts via ssh:
```
users:
  alice:
    - alice
  eve:
    - eve@device1
    - eve@device2
```
Eve can do so with two different `ssh` keys. Alice only with his only SSH Key.


The `files/ssh_public_keys/` contains the following files:

```
alice_ed25519.pub
bob_ed25519.pub
eve@device1_ed25519.pub
eve@device2_ed25519.pub
```

Alice, Bob and Eve want to be users on this host:
```
accounts:
  - alice
  - bob
  - eve
```

Add ssh keys from github user ``DO1JLR`` for local user L3D
```
github_users:
  l3d:
    - do1jlr
```
 Generate ed25519 SSH Keys
--------------------------------

```bash
ssh-keygen -t ed25519
```

