# Stone.Co VSTS Agent

This is an Ansible role to install Azure VSTS Agent on Linux.

## What does this project do?
* install/uninstall vsts-agent on debian/rhel family
* configure vsts-agent (supported modes: Environment/Deployment Group/Agent Pool)
 
## Setup
### Environment Variables
Default values are placed in [`defaults/main.yml`](defaults/main.yml).

**user must choice one agent type and set its variable:**
- vsts_environmentname
- vsts_poolname
- vsts_deploymentgroupname

The agent run as `vsts_agent_user`, by default defined as vstsuser. This user has limited permissions at sudoers, but users can override agent permissions throught `vsts_sudo_permissions` variable.
Ex:
```
vsts_sudo_permissions:
  - /bin/systemctl reload vsts-agent
  - /bin/systemctl status vsts-agent
  - /bin/systemctl start vsts-agent
  - /bin/systemctl restart vsts-agent
  - /bin/systemctl stop vsts-agent
  - /bin/systemctl daemon-reload
  - /bin/bash /home/vstsuser/work/_temp/*.sh # allow execution of scripts generated by bash task.
```

## Intallation
We usually install roles with ansible galaxy.

Adds the role to your `requirements.yml` file
```
- src: git+git@github.com:stone-payments/ansible-role-vsts-agent.git
  version: master
  name: stone-payments.vsts-agent
```
Then run:
```
ansible-galaxy install -r requirements.yml
```

## Playbook Example
```
- hosts: all
  become: true
  roles:
    - role: stone-payments.vsts-agent
      vars:
        vsts_environmentname: "AppName-Production-DC1"
        vsts_sudo_permissions:
          - /bin/rm -rf /opt/apps/*
          - /bin/bash /home/vstsuser/work/_temp/*.sh
          - /bin/systemctl reload vsts-agent
          - /bin/systemctl status vsts-agent
          - /bin/systemctl start vsts-agent
          - /bin/systemctl restart vsts-agent
          - /bin/systemctl stop vsts-agent
          - /bin/systemctl daemon-reload
          - /bin/cp files/newrelic.ini /etc/newrelic.ini
          - /bin/cp files/ucpy@.service /etc/systemd/system/
          - /bin/cp files/ucpy-dl@.service /etc/systemd/system/
          - /bin/cp files/ucpy.target /etc/systemd/system/
          - /bin/systemctl stop ucpy.target
          - /bin/systemctl start ucpy.target
          - /bin/systemctl status ucpy.target
          - /bin/sh /opt/apps/universal-consumer-python/files/enable_target.sh
```

## Contributing
Just open a PR. We love PRs!