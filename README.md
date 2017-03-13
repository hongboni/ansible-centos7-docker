# Provision a Docker Swarm on CentOS 7 server with Ansible
This is a my way of provisioning and locking down a new CentOS 7 hosts

## Requirements

Run this command to install dependencies under ./required-roles

`ansible-galaxy install -r requirements.yml`

## Roles & Tasks

### Dependencies
* [ansible-centos7-common](https://github.com/hongboni/ansible-centos7-common)

### SecureHost play
* Configure SELinux 
* Configure firewalld
* Configure sshd - disbale root & passwd login
* Configure sshd - add sudo user with ssh keys

## Example Plays

Run the 'securehost' playbook against a list of hosts as defined in file ./hosts

Before running next script, you need to make sure that you can log into the remote host as the root user
without password using ssh keys.


```
ansible-playbook securehost.yml 
```

After running above script, you can only log into the remote host via sudo user '*centos*'.
Current host can still access remote host via port 22, 

```
ssh centos@remote.host.ip
```

All other machines can only access the remote hosts via a special port of 22022 
(as defined in ansible-centos7-common role).

```
ssh -p 22022 centos@remote.host.ip
```

## License
BSD
