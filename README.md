[![Ansible Galaxy](https://raw.githubusercontent.com/roles-ansible/ansible_role_auth/main/.github/galaxy.svg?sanitize=true)](https://galaxy.ansible.com/do1jlr/auth) [![MIT License](https://raw.githubusercontent.com/roles-ansible/ansible_role_auth/main/.github/license.svg?sanitize=true)](https://github.com/roles-ansible/ansible_role_auth/blob/main/LICENSE)

 ansible role auth
==============================
Ansible Rolle to manage and deploy ssh keys of admin and non-admin users

 intended use
---------------
This role is designed to manage linux hosts with the following roles. This role here basically only focuses on deploying the correct ssh public keys to the correct users depending on the configuration.
Other roles icreating users and groups, configure sshd, roll out dotfiles or install a number of useful packages.

A list of suggested roles to manage your linux host:
 - [do1jlr.base](https://github.com/roles-ansible/ansible_role_base.git) *install some useful packages*
 - [do1jlr.users](https://github.com/roles-ansible/ansible_role_users.git) *create user and manage sudoers*
 - [do1jlr.auth](https://github.com/roles-ansible/ansible_role_auth.git) *(this one)*
 - [do1jlr.sshd](https://github.com/roles-ansible/ansible_role_sshd.git) *configure sshd*
 - [do1jlr.dotfiles](https://github.com/roles-ansible/ansible_role_dotfiles) *deploy some fancy dotfiles*

 Good to know:
---------------
The listed roles use the same variables to create accounts, admins and so on. But the roles have to run in the correct order to work properly.
For example you can't deploy a ssh public key for a user that is not created.

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

