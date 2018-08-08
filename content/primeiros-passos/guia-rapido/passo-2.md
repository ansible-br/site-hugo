+++
title = "Passo 2 - Instalação"
description = "Instalação do Ansible"
weight = 3
+++
## Instalando o Ansible Control

A instalação do Ansible em ambiente RHEL/CentOS 6+ é realizada da mesma forma que no Fedora 24+ , mudando só o utilitário gerenciador de pacotes de DNF para YUM:

#### RHEL/Centos 6+

```bash
$ sudo yum install ansible libselinux-python -y
```

#### Fedora 24+

```bash
$ sudo dnf install ansible libselinux-python -y
```

{{% alert theme="info" %}} Para outras opções de instalação do Ansible, [siga essas instruções (inglês)](http://docs.ansible.com/ansible/latest/intro_installation.html){{% /alert %}}
