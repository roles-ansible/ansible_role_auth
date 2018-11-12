 role-ssh_authorized_keys
==============================

Ansible Rolle für die SSH Keys

 Beispiel Konfiguration:
------------

```bash
/host_vars/gw01.ffbsee.net
--------------------------
admins:
  - mart
  - l3d

admin_ssh_keys: 'admin_ssh_keys'
```
```
/files/admin_ssh_keys/mart_id.pub
--------------------------------
SSH-Public Key
```

```
/files/admin_ssh_keys/l3d_id.pub
--------------------------------
SSH-Public Key
```

