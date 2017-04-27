# deleteuponrecognition - Docker Swarm with Ansible

## Technolgies Used
- [Vagrant](https://www.vagrantup.com/)
- [VirtualBox](https://www.virtualbox.org/wiki/VirtualBox)
- [Docker](https://www.docker.com/)
- [Ansible](https://www.ansible.com/)

## What Does It Do?
- Creates 3 (configurable) centos/7 Virtual Machines on the local machine using Vagrant + VirtualBox
- Uses Ansible to configure those machines and install Docker
- Also via Ansible, designates one Docker instance as the manager, two Docker instances as workers, and creates a Docker Swarm

## How To Run It

1. Install Vagrant, VirtualBox, and Ansible locally
1. Check out the project locally
1. From within the `swarm` directory, execute the following
```bash
vagrant up
```

If everything goes according to plan:
- Vagrant will download the centos/7 box image (could take awhile the first time)
- Vagrant will start 3 VMs (configurable within `machines.yml`)
- Vagrant will trigger the `provisioning/playbook.yml` Ansible Playbook which will:
  - Install Docker on each machine in parallel
  - Start the Docker swarm from the first manager defined
  - Add any additional manages to the swarm (none defined by default)
  - Add workers to the swarm

To access your swarn, ssh into the manager VM:
```bash
swarm$ vagrant ssh host-01
```
Now run docker commands from within the manager VM:
```bash
$ docker info

Containers: 0
Running: 0
Paused: 0
Stopped: 0
  ...snip...
Swarm: active
  NodeID: hz2vwlv5415o8ddnwviawya91
  Is Manager: true
  Managers: 1
  Nodes: 3
  ...snip...
```
```bash
$ docker node ls
ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
41yugm9jjx1fnvrrf1ojco7ok    host-02   Ready   Active        
hz2vwlv5415o8ddnwviawya91 *  host-01   Ready   Active        Leader
wi986agsm5u4ph8u3pt2xzgjk    host-03   Ready   Active        
```

For more information about interacting with the swarm and deploying services, see [here](https://docs.docker.com/engine/swarm/swarm-tutorial/deploy-service/).
