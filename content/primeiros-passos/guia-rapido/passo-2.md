+++
title = "Passo 2 - Instalação"
description = "Instalação do Ansible"
weight = 3
+++
## Instalando o Ansible Control:

A instalação do RHEL/CentOS 6+ (yum) segue o mesmo procedimento para o Fedora 24+ (dnf), mudando só o utilitário gerenciador de pacotes (yum-->dnf):

**RHEL/Centos 6+**

	$ sudo yum install ansible libselinux-python -y

**Fedora 24+**

	$ sudo dnf install ansible libselinux-python -y

{{% alert theme="info" %}} Para outras opções de instalação do Ansible, [siga essas instruções (inglês)](http://docs.ansible.com/ansible/latest/intro_installation.html){{% /alert %}}
