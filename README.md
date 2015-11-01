Generic dev box

## Preparation

Copy the config:

```
cp box.yml{.sample,}
```

Edit the `box.yml` to pick your own settings for the virtual machine. `vagrant up` to start the VM.

```
ssh-add /path/to/your/private_key
ssh-copy-id -i /path/to/your/public_key.pub vagrant@ip.of.vagrant.box
```

`vagrant` user's password is `vagrant`.

## Hosts file

Copy `development.sample` to `development`:

```
cp development{.sample,}
```

Then edit the file to point to the IP of the virtual machine.

## Running Playbooks

All of the playbooks below are run like this:

```
ansible-playbook <playbook-name>.yml -i development
```

### Setup

This should always be run.

Playbook name: `setup`

### Ember

Playbook name: `ember`
