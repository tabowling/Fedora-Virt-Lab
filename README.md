# Fedora-Virt-Lab
A demo for setting up a Fedora hypervisor for your home lab using Ansible, Web Console, and Image Builder

Instructions available at https://github.com/tabowling/Fedora-Virt-Lab
This lab assumes you are running locally on a Fedora host.


Playbooks to setup Image Builder quickly with OSBuild backend:
```
yum install -y ansible linux-system-roles
ansible-galaxy install linux-system-roles.firewall \
                       linux-system-roles.cockpit \
                       linux-system-roles.image_builder

Add hosts to /etc/ansible/hosts

git clone https://github.com/tabowling/Fedora-Virt-Lab.git
cd Fedora-Virt-lab
ansible-playbook -c local -l localhost virt-host.yml

```
Download the pre-built Fedora Cloud image to /VirtualMachines/
```
virt-customize -a disk.img \
        --network \
        --hostname fedora-template \
        --root-password file:/root/.creds
        --ssh-inject root:file:/home/tbowling/.ssh/id_rsa_demo.pub
```

To manually install packages and enable the service with OSBuild backend:
```
yum install -y osbuild osbuild-composer cockpit-composer composer-cli
systemctl enable osbuild-composer.service --now
```

To manually install packages and enable the service with Lorax backend:
```
yum install -y lorax-composer cockpit-composer composer-cli
systemctl enable lorax-composer.service --now
```

Create your own custom OS image with Image Builder
Copy from /var/lib/osbuild-composer/artifacts/<UUID>/disk.qcow2 to /VirtualMachines


https://weldr.io/lorax/lorax-composer.html#blueprints
https://www.osbuild.org/
https://alt.fedoraproject.org/cloud/
