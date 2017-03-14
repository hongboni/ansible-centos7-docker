# Provision a Docker Swarm on CentOS 7 server with Ansible

This playbook will first provision and lock down all hosts, then setup
a Docker swarm with managers and workers.

## Requirements

Run this command to install dependencies under ./required-roles

`ansible-galaxy install -r requirements.yml`

## Roles & Tasks

### Dependencies
* [ansible-centos7-common](https://github.com/hongboni/ansible-centos7-common)

### Docker-Swarm play
* Configure SELinux, firewalld and sshd on all hosts
* Configure Docker on all hosts
* Configure Docker Swarm Manager on all manger hosts
* Configure Docker Swarm Workers on all worker hosts

## Example Usage

### Setup inventory in ./hosts
* Make sure that you can log into all remote hosts as the root user without password using ssh keys.
* Add host ip in file ./hosts for all Docker swarm Managers and Workers

### Provision with ansible

```
ansible-playbook -i hosts site.yml 
```

After running above script, you can only log into the remote host via sudo user '*centos*'.
Current host can still access remote host via port 22, 

```
ssh centos@192.168.1.110
```

All other machines can only access the remote hosts via a special port of 22022 
(as defined in ansible-centos7-common role).

```
ssh -p 22022 centos@192.168.1.110
```

Once you are login, you can run "sudo su" to become root user.

## Manage Docker Swarm

### Deploy a service to the swarm
Login to one of the Manager nodes, and become root with 'sudo su', then issue the follow
command to create a sample Service:

``` 
docker service create --replicas 1 --name helloworld alpine ping docker.com
```

### Inspect a service on the swarm
```
docker service inspect --pretty helloworld
```

### Scale Service to N copies (replicas)

```
docker service scale helloworld=5
```

### Remove a service

```
docker service rm helloworld
```

### More Info

[Swarm mode overview](https://docs.docker.com/engine/swarm/)


## License
BSD
