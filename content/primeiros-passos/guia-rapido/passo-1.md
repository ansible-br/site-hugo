+++
title = "Passo 1 - Repositório"
description = "Adicionando repositorio EPEL para CentOS/RHEL 6"
weight = 2
+++

## Adicionando o repositório do EPEL:

{{% notice warning %}}
Para o Fedora 24+ ou RHEL/CentOS 7 não será necessário este passo. O Ansible está disponível no repositório padrão.
{{% /notice %}}

Para os ambientes de RHEL/CentOS 6, é necessário adicionar/instalar o repositório do EPEL (Extra Packages for Enterprise Linux).


	$ sudo yum install epel-release -y
