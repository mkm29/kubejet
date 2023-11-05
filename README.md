# RKE2 Automation With JetPorch

![KubeJet](media/kubejet.png)

## Prerequisites

- [JetPorch](https://www.jetporch.com/)
  - I use Cargo to install: `cargo install jetp`

### ssh-agent

Since JetPorch uses SSH to connect to the nodes, you will need to add your SSH key to the ssh-agent.

```
ssh-add ~/.ssh/rke2-controlplane
ssh-add ~/.ssh/rke2-worker
# verify
ssh-add -l
```

### sudo

Certain tasks must be run as the root user, so you need to add the user on the VM to the wheel group. You also need to disable the requirement for a password when using sud, so add this to `visudo` (replace `master` with your username):

```
master ALL=(ALL) NOPASSWD: ALL
```

## Structure

```console
├── inventory
│   ├── groups
│   │   └── controlplanes
│   └── group_vars
│       ├── controlplanes
│       └── workers
├── media
│   └── kubejet.png
├── playbooks
│   └── install_jetporch.yaml
└── README.md
```

### Inventory

Here we have created 2 groups: controlplanes and workers. Within the inventory directory, create a groups subdirectory and a group_vars subdirectory. Within the groups subdirectory, create a file for each group. Within the group_vars subdirectory, create a file for each group. The group_vars files will contain the variables that will be used in the playbook, such as SSH connection information.

## To run playbook

```
jetp ssh --playbook playbooks/install_jetporch.yaml --inventory ./inventory -vvv

```