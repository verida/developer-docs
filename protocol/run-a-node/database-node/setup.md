# Setup

## Requirements

{% hint style="info" %}
Verida Data Nodes are currently tested and validated on Ubuntu 24.04 LTS (Noble) Minimal, on x86-64 machines.
{% endhint %}

### Hardware Requirements

A small verida node can run on a VM with as little as 2GB of RAM and a single core. This is sufficient for light casual use by a few hundred users.

The most important consideration in determining hardware requirements is the number of user operations per-second the node will support.

Verida Mainnet storage nodes are 4 core 8 GB VMs.

### Software Requirements

We expect a virtual machine running `Ubuntu 24.04 LTS (Noble) Minimal`. For the major cloud providers here are some we have tested:

* AWS
  * AMI `ami-0e86e20dae9224db8`
* Google Cloud Platform (GCP):
  * Version: `Ubuntu 24.04 Minimal`
  * `x86/64, amd64 noble minimal image (latest build)`
* MS Azure:
  * publisher: `canonical`,
  * offer: `0001-com-ubuntu-minimal-jammy`,
  * sku: `minimal-24_04-lts-gen2`

We need the following inbound ports open: 22 (or any port for ssh), 443 (https), 80 (http)

* SSH is needed during setup. You manually SSH to the machine so can choose any port. 22 is the default.
* We run all web services using HTTP on port 443
* Port 80 is used during the setup phase by LetsEncrypt to provision a HTTPS certificate.

You will need a domain name pointing to the node.

## Quickstart

{% hint style="info" %}
This process works on an Ubuntu 24.04 Minimal LTS machine.

It does the following:

* Installs `nodejs` and `npm` on the machine.
* Installs the `@verida\verida-node-installer` node script.
* You can inspect the script [here](https://github.com/verida/storage-node-operators/blob/main/setup/installer/install.sh).
{% endhint %}

Run the following and follow the on-screen instructions:

```
curl -s -L https://assets.verida.io/datanodeinstaller/install.sh | bash
```

Securely backup the `./<output_dir>/verida-node-config.yaml` and `./<output_dir>/verida-node-config.tgz` files and then follow the instructions from the setup tool.

{% hint style="danger" %}
The `verida-node-config.yaml` file contains critical information (including private keys) for running your node. It is vital you keep this secure. `verida-node-config.tgz` contains a copy of `verida-node-config.yaml` as well as all other config files setup for your machine.
{% endhint %}

{% hint style="info" %}
You can setup a new machine using the same configuration with this command:

```
setup-verida-node --inputconfig verida-node-config.yaml
```
{% endhint %}

Once the setup process is complete, and you have verified you have safely backed up the setup files you should delete them from the machine.

{% hint style="info" %}
You should delete these setup files because they contain copies of your private keys. These keys are also in the config files in `/var/lib/verida` but it is best to minimize places they are kept.
{% endhint %}

On the VM run:

```
# if setupfiles is the directory your setup files are in 
rm -rf ./setupfiles
```

### Video Quickstart

{% embed url="https://youtu.be/sFijq1ANfNc" %}

### How it works

This installer will configure three docker images using Docker Compose. The three images are:

* HAProxy: HTTPS termination and front end
* CouchDB: Data Storage
* Verida Storage Node: Authentication and Authorization layer over CouchDB

The installer will ask where you want to install to. By default this is `/var/lib/verida`. When setting up your node please consider mounting this on a separate drive as `/var/lib/verida/data` can get large.

{% hint style="info" %}
`/var/lib/verida/data` is the default directory for data storage. `/var/lib/verida/backups`is used as temporary space during the backup process.
{% endhint %}

Assuming this default, important directories to note:

* `/var/lib/verida/data` is where data is stored.
* `/var/lib/verida/etc` has subdirectories for config files for CouchDB and HAProxy
* `/var/lib/verida/dockercompose` is the working directory for Docker Compose. The `docker-compose.yml` file is here which contains important configuration for all nodes.
* `/var/lib/verida/bin` contains operational scripts for the node.
  * `refresh_verida_docker.sh` will download the latest Verida Docker image and restart docker
  * See the [section on backups](broken-reference) for details of `run_backup.sh` and `move_backup.sh`
* `/var/lib/verida/backups` is where the backup script puts your backups.

###

### Manually setting up a node yourself

You are welcome to setup a node manually yourself. We suggest running the setup tool first and inspecting the script and config files generated.

The template scripts the setup tool uses may also be useful and can be [found on Github](https://github.com/verida/data-node-operators/tree/develop/setup/templates).

## Next Steps

* [Join our Discord ](https://discord.gg/AzcRJvvWEF)for support and to get critical updates.
* See [Operating a Verida Node](operations.md) for things to consider when running a node.
