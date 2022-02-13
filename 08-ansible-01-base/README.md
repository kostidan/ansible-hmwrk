# Домашнее задание к занятию "08.01 Введение в Ansible"

## Основная часть
1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте какое значение имеет факт `some_fact` для указанного хоста при выполнении playbook'a.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] **********************************************************************************

TASK [Gathering Facts] *********************************************************************************
ok: [localhost]

TASK [Print OS] ****************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************************************
ok: [localhost] => {
    "msg": "12"
}

PLAY RECAP *********************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# cat group_vars/all/examp.yml
---
  some_fact: "all default fact"
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```

```
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-playbook -i inventory/test.yml 
site.yml

PLAY [Print os facts] **********************************************************************************

TASK [Gathering Facts] *********************************************************************************
ok: [localhost]

TASK [Print OS] ****************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP *********************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```
3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# docker ps
CONTAINER ID   IMAGE                 COMMAND               CREATED             STATUS             PORTS     NAMES
a8ad535e5895   pycontribs/ubuntu     "sleep 60000000000"   55 minutes ago      Up 55 minutes                ubuntu
1ee08d5dcfc4   pycontribs/centos:7   "sleep 60000000000"   About an hour ago   Up About an hour             centos7
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```

4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] **********************************************************************************

TASK [Gathering Facts] *********************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************************************
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}

TASK [Print fact] **************************************************************************************
ok: [ubuntu] => {
    "msg": "deb"
}
ok: [centos7] => {
    "msg": "el"
}

PLAY RECAP *********************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```

5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились следующие значения: для `deb` - 'deb default fact', для `el` - 'el default fact'.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# cat group_vars/deb/examp.yml
---
  some_fact: "deb default fact"
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# cat group_vars/el/examp.yml
---
  some_fact: "el default fact"
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```

6. Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-playbook -i inventory/prod.yml site.yml

PLAY [Print os facts] **********************************************************************************

TASK [Gathering Facts] *********************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************************************
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}

TASK [Print fact] **************************************************************************************
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}

PLAY RECAP *********************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```

7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-vault encrypt group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-vault encrypt group_vars/el/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
```
8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] **********************************************************************************

TASK [Gathering Facts] *********************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *********************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```
9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-doc -t connection -l
[WARNING]: Collection frr.frr does not support Ansible version 2.11.6
[WARNING]: Collection ibm.qradar does not support Ansible version 2.11.6
[WARNING]: Collection splunk.es does not support Ansible version 2.11.6
ansible.netcommon.httpapi      Use httpapi to run command on network appliances
ansible.netcommon.libssh       (Tech preview) Run tasks using libssh for ssh connection
ansible.netcommon.napalm       Provides persistent connection using NAPALM
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol
ansible.netcommon.network_cli  Use network_cli to run command on network appliances
ansible.netcommon.persistent   Use a persistent unix socket for connection
community.aws.aws_ssm          execute via AWS Systems Manager
community.docker.docker        Run tasks in docker containers
community.docker.docker_api    Run tasks in docker containers
community.docker.nsenter       execute on host running controller container
community.general.chroot       Interact with local chroot
community.general.funcd        Use funcd to connect to target
community.general.iocage       Run tasks in iocage jails
community.general.jail         Run tasks in jails
community.general.lxc          Run tasks in lxc containers via lxc python library
community.general.lxd          Run tasks in lxc containers via lxc CLI
community.general.qubes        Interact with an existing QubesOS AppVM
community.general.saltstack    Allow ansible to piggyback on salt minions
community.general.zone         Run tasks in a zone instance
community.kubernetes.kubectl   Execute tasks in pods running on Kubernetes
community.libvirt.libvirt_lxc  Run tasks in lxc containers via libvirt
community.libvirt.libvirt_qemu Run tasks on libvirt/qemu virtual machines
community.okd.oc               Execute tasks in pods running on OpenShift
community.vmware.vmware_tools  Execute tasks inside a VM via VMware Tools
containers.podman.buildah      Interact with an existing buildah container
containers.podman.podman       Interact with an existing podman container
kubernetes.core.kubectl        Execute tasks in pods running on Kubernetes
local                          execute on controller
paramiko_ssh                   Run tasks via python ssh (paramiko)
psrp                           Run tasks over Microsoft PowerShell Remoting Protocol
ssh                            connect via ssh client binary
winrm                          Run tasks over Microsoft's WinRM
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```
```buildoutcfg
local                          execute on controller
```
10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# cat inventory/prod.yml
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#

```
11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь что факты `some_fact` для каждого из хостов определены из верных `group_vars`.
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# cat group_vars/local/exampl.yml
---
  some_fact: "local default fact"
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```
```buildoutcfg
root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password:

PLAY [Print os facts] **********************************************************************************

TASK [Gathering Facts] *********************************************************************************
ok: [localhost]
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************************************
ok: [localhost] => {
    "msg": "local default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *********************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

root@vagrant:/home/vagrant/ansible-hmwrk/08-ansible-01-base/playbook#
```
