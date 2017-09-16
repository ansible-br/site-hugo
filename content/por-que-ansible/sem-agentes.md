+++
title = "Sem agentes"
description = "Evite o problema de **gerenciar o gerencimento**, comum em vários sistemas de automação."
weight = 1
+++

## Somente um “Ansible Control”:

Evite o problema de **"gerenciar o gerencimento"**, comum em vários sistemas de automação.

Ansible não necessita de agentes instalados nas pontas. A única necessidade é um servidor com Ansible instalado **(Ansible Control)** com acesso aos servidores a serem administrados **(hosts)** através dos protocolos **SSH** (para ambientes Linux) ou **WINRM** (Acesso remoto Windows) . As Playbooks empurram (push) as configurações desejadas nos hosts definidos no inventário e podem inclusive ser executadas **Ad-Hoc** (via linha de comando sem a necessidade de definições em arquivos). O modelo sem agente efetua ajustes e comunicação mais rapidamente do que no modelo cliente-servidor.

O único requisito nos **hosts** Linux é ter o **Python** instalado. Hoje sabemos que o **Python** já vem instalado nativamente na maioria das distribuições Linux.


{{%children style="h2" description="true"%}}
