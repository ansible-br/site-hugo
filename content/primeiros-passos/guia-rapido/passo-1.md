+++
title = "Passo 1 - Repositório"
description = "Adicionando repositório EPEL para CentOS/RHEL 6"
weight = 2
+++

## Adicionando o repositório do EPEL

{{% notice warning %}}
Para o Fedora 24+ ou RHEL/CentOS 7 não será necessário executar este passo. O Ansible já está disponível no repositório padrão.
{{% /notice %}}

Para os ambientes de RHEL/CentOS 6, é necessário adicionar/instalar o repositório do EPEL (Extra Packages for Enterprise Linux).

```bash
$ sudo yum install epel-release -y
```
