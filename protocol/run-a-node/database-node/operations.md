# Operations

## Critical files

The file `verida-node-config.yaml` is created by the installer and contains all your keys and passwords for the node. **These values are extremely sensitive**. This file should be securely backed up.

These values are stored in:

* `verida-node-config.yaml` in the config directory created by the installed. This can be removed once backed up.
* The compressed `verida-node-config.tgz` in the config directory created by the installed. This can be removed once backed up.
* Inside `/var/lib/verida` where they are required by the running process. These **cannot be removed**.

{% hint style="danger" %}
Only you have access to the keys in`verida-node-config.yaml`. Verida is unable to recover these values in any way. `verida-node-config.tgz` also conatins this file as well as other generated configuration scripts.
{% endhint %}

## Backups

{% hint style="info" %}
`/var/lib/verida/data` is the default directory for data storage. `/var/lib/verida/backups`is used as temporary space during the backup process.
{% endhint %}

The `/var/lib/verida/bin/run_backup.sh` backup script is created and scheduled using cron. By default backups are placed in `/var/lib/verida/backups`.

* After each backup `/var/lib/verida/bin/move_backup.sh` is run. You should edit this script to move the backup to a safe, off machine location.
* Look in [`operations_scripts`](https://github.com/verida/data-node-operators/tree/develop/setup/operations\_scripts) on Github for sample scripts for various cloud providers.
* This backup script provides basic backups that should operate everywhere. If you customise it please check the order files are backuped up carefully - the order they are done in the script is important for CouchDB operations. File system snapshots are also suitable.
* The backup restoration process is documented below.

### Restoring a node from backups

Restoring or rebuilding a node from a backup consists of three parts:

1. Setup a VM and restore the encrypted database
2. Recreate configuration with the same encryption keys as before.
3. Apply the configuration to the machine

#### Setup a VM

Setup your VM using your choice of tools. On this VM run the following:

```
sudo mkdir -p /var/lib/verida/data
sudo chown -R ubuntu:ubuntu /var/lib/verida/data
```

Next copy the backup `.tgz` file to `/var/lib/verida/data` on the VM, and decompress it (eg `tar -zxf backupfile.tgz`).

#### Recreate configuration

To recreate the configuration using the backup of the `verida-node-config.yaml` first restore the file to your local system. Next run:

```
setup-verida-node --inputconfig verida-node-config.yaml
```

#### Apply the configuration to the machine

To apply the configuration simply follow the instructions given by the setup script. Once completed, you can visit the `/status` URL on your node and check the number of users is correct.

## Web Services

You can visit `https://your-node-name.example.com/status` to see some public infomation about your node.

HAProxy is set to restarted 4 times a day. We've found this maximises reliability.
