+++
title = "Windows - Pré-requisitos"
description = "Configurações necessárias para utilizar Ansible com Windows"
weight = 2
+++

## Configurando um Host Windows

A configuração de um host Windows para ser gerenciado com Ansible se concentra em três componentes:

1. .Net Framework
2. Windows Management Framework
3. Configuração WinRM

### Versões suportadas do Windows

O Ansible suporta todas as versões atuais e com suporte estendido do Windows.

* Desktop: 7, 8.1 e 10
* Server: 2008, 2008 R2, 2012, 2012 R2 e 2016

### .Net Framework

É necessário que a versão 4 do .Net Framework esteja instalada no servidor a ser gerenciado. A instalação é realizada através do Server Manager pela funcionalidade "Add and Remove Windows Features", pelo *cmdlet* PowerShell `Install-WindowsFeature` ou pelo utilitário CLI dism.exe.

### Versão do Windows Management Framework (WMF)

É necessário que o WMF esteja, no mínimo, na versão 3.0. A versão mais atual disponível no momento é a 5.1, sendo esta a versão nativa das edições 10 e Server 2016 do Windows.  
No caso das outras edições é necessário instalar uma atualização para ter acesso aos novos recursos do WMF, como o PowerShell e o Desired State Configuration (DSC).

{{% notice warning %}}
Antes de atualizar o WMF, deve-se verificar se alguma aplicação tem problemas de compatibilidade com a versão alvo da atualização. Alguns produtos como Microsoft Exchange e Microsoft SQL Server possuem requerimentos de versão do WMF que podem impedir a sua atualização.
{{% /notice %}}

### Configuração WinRM

Para que o Ansible consiga se conectar a um servidor Windows é necessário que haja um *listener* WinRM configurado para receber comandos remotos.  
Está disponível no repositório do Ansible o script PowerShell [ConfigureRemotingForAnsible.ps1](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1), que se encarrega de criar o *listener*, fazendo os ajustes necessários. Explicaremos abaixo as principais tarefas que o script realiza.  
Nos casos de ambientes que façam uso do Active Directory Domain Services (ADDS ou simplesmente AD), a configuração dos *listeners* também pode ser feita através de *Group Policy Objects*.

#### Configuração WinRM: Configurações de autenticação

O WinRM permite várias opções de autenticação. Na [documentação oficial do Ansible](https://docs.ansible.com/ansible/2.6/user_guide/windows_winrm.html) há informações e recomendações para cada opção.  
Nos servidores que façam parte de um domínio do ADDS, as opções de autenticação mais comuns serão Negotiate(NTLM), CredSSP e Kerberos. Por ser um protocolo mais antigo e não permitir a delegação de credenciais, o NTLM deve ser preterido em favor do Kerberos e CredSSP.

#### Configuração WinRM: Script ConfigureRemotingForAnsible.ps1

O script PowerShell ConfigureRemotingForAnsible.ps1 realiza a configuração de um listener do WinRM, a fim de possibilitar o acesso remoto ao servidor.  
Por padrão, o script executará o seguinte:

1. Criar um certificado auto-assinado no vault do computador
    1. O certificado terá o nome do computador sem sufixo DNS
    2. Validade de 1095 dias (3 anos)
    3. Chave de 4096 bits
    4. Assinatura SHA256
2. Alterar ou criar regra no firewall para permitir o tráfego de gerência remota
3. Habilita o PowerShell Remoting
4. Configura o serviço WinRM para iniciar automaticamente
5. Criar o listener WinRM
    1. Desabilita autenticação básica.
    2. HTTPS como protocolo de transporte
    3. Define o certificado auto-assinado no listener
    4. Faz um teste básico de conexão remota utilizando PowerShell Remoting.

Após a execução do script, o servidor já estará acessível remotamente. Segue um exemplo de inventário para testar o acesso utilizando o módulo `win_ping`. Suponha um servidor Windows `winsrv`, membro de um domínio AD `ansible-br.org` em que o usuário Ansiovaldo tenha privilégios de administração. Utilizamos a versão 2.6 do Ansible.

##### Inventário

```ini
# Inventário Windows

[windows]
winsrv

[windows:vars]
ansible_connection = "winrm"
ansible_port = 5986
ansible_winrm_transport = "kerberos"
ansible_user = "ansiovaldo@ANSIBLE-BR.ORG" # é necessário informar o usuário no formato de User Principal Name (UPN)
ansible_password = "minha-senha-que-nao-deveria-estar-em-plaintext" # prefira informar durante a execução ou colocar em um vault. Utilizamos esta opção para manter o exemplo simples.
ansible_winrm_server_cert_validation = "ignore" # como estamos utilizando um certificado auto-assinado, é necessário desabilitar a verificação de certificado. No Python 2.7.9+, por padrão, o certificado é verificado.

```

##### Executando o comando

```bash
$ ansible winsrv -i inventory -m win_ping

 _______________________
< PLAY [Ansible Ad-Hoc] >
 -----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

 _________________
< TASK [win_ping] >
 -----------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

ok: [winsrv]
 ____________
< PLAY RECAP >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

winsrv                   : ok=1    changed=0    unreachable=0    failed=0


```

[//]: # (TODO: Colocar alteração da chave de registro HTTPS para tokens grandes)

