+++
title = "Windows - Conceitos Básicos"
description = "Conceitos básicos do uso de Ansible com Windows"
weight = 2
+++

## Windows & Ansible - Conceitos básicos

Há algumas diferenças na utilização de Ansible para gerenciar servidores Windows quando comparados com os servidores Linux.

### Forma de Conexão

A primeira diferença é na forma de conexão entre o Ansible Control e o host. É utilizado o Windows Remote Management (WinRM), que é a implementação da Microsoft do Web Services Management (WS-Man), um padrão aberto do Distributed Management Task Force (DMTF), em que se utiliza um web service baseado em SOAP para gerência remota de diversos dispositivos.[^1]  
Recentemente, a Microsoft disponibilizou uma implementação do OpenSSH Client e Server nativa para Windows 10 e Server 2016[^2]. No PowerShell Core é possível utilizar o SSH para estabelecer sessões remotas.[^3] Entretanto, ainda não há suporte dessa funcionalidade no Ansible.


### PowerShell

A segunda diferença é o uso de scripts PowerShell no lugar do Python na execução das tasks nos hosts. Ao gerenciar servidores Linux, o Ansible converte as playbooks em scripts Python que são transferidos via SSH para os hosts e executados remotamente.  
No caso do Windows, são gerados scripts PowerShell que são transferidos via WinRM (utilizando a biblioteca pywinrm), para então serem executados nos hosts.


[^1]: [Wikipedia: WS-Management](https://en.wikipedia.org/wiki/WS-Management)
[^2]: [PowerShell Team Blog: Using the OpenSSH Beta in Windows 10 Fall Creators Update and Windows Server 1709](https://blogs.msdn.microsoft.com/powershell/2017/12/15/using-the-openssh-beta-in-windows-10-fall-creators-update-and-windows-server-1709/)
[^3]: [Microsoft Docs: Comunicação remota do PowerShell por SSH](https://docs.microsoft.com/pt-br/powershell/scripting/core-powershell/ssh-remoting-in-powershell-core?view=powershell-6)