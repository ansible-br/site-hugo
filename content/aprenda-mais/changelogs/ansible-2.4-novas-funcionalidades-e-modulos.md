+++
title = "Ansible 2.4 — Novas funcionalidades e Módulos"
description = "Destrinchando as novas funcionalidades do Ansible 2.4"
weight = 1
+++

## O que há de novo?

A seguir uma série de funcionalidades e novos módulos que vieram nesta nova versão 2.4:


### Modernização:

O Ansible agora suporta plenamente o Python 3 bem como o inventário no formato baseado em YAML.

### Playbooks:

Diretiva `order`: Agora é possível modificar a ordem que o Ansible processa os hosts quando dispara as tasks.

    - hosts: all
      order: sorted
      gather_facts: False

      tasks:
        - debug: var=inventory_hostname

Possíveis valores para a diretiva `order`:

{{%alert info%}}
**inventory:**
O padrão. A ordem conforme inserção no arquivo de inventário

**reverse_inventory:**
Como o nome já diz, reverte a ordem de inserção do arquivo de inventário.

**sorted:**
Os Hosts são ordenados alfabeticamente pelo nome.

**reverse_sorted:**
Os Hosts são ordenados alfabeticamente invertidos pelo nome.

**shuffle:**
Os Hosts são aleatoriamente ordenados a cada execução.
{{%/alert%}}

### Inventário:

  + Agora é possível especificar múltiplos arquivos de inventário na linha de comando:

  <b></b>

    $ ansible-playbook -i /etc/hosts1 -i /opt/hosts2 play.yml

  * Novos Plugins de inventário: [**host_list**](https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/inventory/host_list.py) e [**advanced_host_list**](https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/inventory/advanced_host_list.py): com ele é possível executar comandos **sem arquivo de inventário previamente definido:**

*Diretamente com IPs:*

    $ ansible -i ‘10.10.2.6, 10.10.2.4’ -m ping all

*Com nomes DNS:*

    $ ansible -i ‘host1.example.com, host2’ -m user -a ‘name=me state=absent’ all

*Para o localhost:*

    $ ansible-playbook -i ‘localhost,’ play.yml -c local

*Modo avançado:*

    $ ansible -i ‘host[1:10],’ -m ping

  * Formato [**YAML para o inventário**](https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/inventory/yaml.py). Modelo:

  <b></b>

      all: # a chave deve ser única, somente um 'hosts' por grupo
          hosts:
              test1:
              test2:
                  var1: value1
          vars:
              group_var1: value2
          children:   # ordem da chave não é importante, indentação sim
              other_group:
                  children:
                      group_x:
                          hosts:
                              test5
                  vars:
                      g2_var2: value3
                  hosts:
                      test4:
                          ansible_host: 127.0.0.1
              last_group:
                  hosts:
                      test1 # mesmo host como acima, membro adicional de um grupo
                  vars:
                      last_var: MYVALUE


### Escalar privilégios no Windows:

Método `runas` está disponível para ambientes Windows. Agora o Ansible trabalha com todos os tipos de autenticação e irá auto elevar sob UAC (controle da conta de usuário) se WinRM for usado como privilégio “Atuar como parte do sistema operacional”.

    --become-method=BECOME_METHOD
                            privilege escalation method to use  
                            (default=sudo),
                            valid choices: [ sudo | su | pbrun | pfexec
                            | doas | dzdo | ksu | runas | pmrun ]

### Filtros:
  * Plugin de Filtros [**hiera**](https://docs.ansible.com/ansible/devel/plugins/lookup/hiera.html): Agora é possível obter dados de arquivos hiera do Puppet. Exemplo:

  <b></b>

    - name: "a value from Hiera 'DB'"
      debug: msg={{ lookup('hiera', 'foo') }}


### Módulos:

  * Módulo de pacotes [**yum**](http://docs.ansible.com/ansible/latest/yum_module.html): Este módulo tem um novo parâmetro `security` para limitar a atualização somente dos pacotes de segurança:

  <b></b>

    - name: upgrade all packages
      yum:
        name: '*'
        state: latest
        security: yes

  * Módulo de comando [**telnet**](http://docs.ansible.com/ansible/latest/telnet_module.html): Finalmente o módulo de rede que faltava no Ansible. É muito comum os dispositivos de rede não terem o ssh habilitado. Com esse módulo é possível habilitá-los através do telnet ou executar diversos outros comandos:

  <b></b>

      - name: send configuration commands to IOS
        telnet:
          user: cisco
          password: cisco
          login_prompt: "Username: "
          prompts:
            - "[>|#]"
          commands:
            - terminal length 0
            - configure terminal
            - hostname ios01


  * Módulo de arquivos [**xml**](http://docs.ansible.com/ansible/latest/xml_module.html): Faltava esse módulo nativamente no Ansible para tratamento de arquivos XMLs:

  <b></b>

    - name: Remove all children from the website element (option 1)
      xml:
        path: /foo/bar.xml
        xpath: /business/website/*
        state: absent

  * Módulo de sistema [**interfaces_file**](http://docs.ansible.com/ansible/latest/interfaces_file_module.html): Gerencie as interfaces de rede de servidores Linux diretamente nos arquivos de configuração. Adicione, remova ou gerencie opções individuais"

  <b></b>

    # Set eth1 mtu configuration value to 8000
    - interfaces_file:
        dest: /etc/network/interfaces.d/eth1.cfg
        iface: eth1
        option: mtu
        value: 8000
        backup: yes
        state: present
      register: eth1_cfg

  * Módulo Windows [**win_domain_group**](http://docs.ansible.com/ansible/latest/win_domain_group_module.html): Gerencie grupos do MS Active Diretory:

  <b></b>

    - name: ensure the group Cow does't exist using the Distinguished Name
      win_domain_group:
        name: CN=Cow,OU=groups,DC=ansible,DC=local
        state: absent

  * Módulo Windows [**win_domain_user**](http://docs.ansible.com/ansible/latest/win_domain_user_module.html): Gerencie usuários do MS Active Diretory:

  <b></b>

    - name: Ensure user bob is present with address information
      win_domain_user:
        name: bob
        firstname: Bob
        surname: Smith
        company: BobCo
        password: B0bP4ssw0rd
        state: present
        groups:
          - Domain Users
        street: 123 4th St.
        city: Sometown
        state_province: IN
        postal_code: 12345
        country: US

  * Módulo Windows [**win_dsc**](http://docs.ansible.com/ansible/latest/win_dsc_module.html): Os scripts de Configuração do PowerShell Desired State Configuration (DSC — um Ansible by Microsoft?) podem ser chamados através deste módulo. Com isso é possível extender suporte do Ansible em ambientes Windows, aproveitando essa arquitetura disponível no Powershell 4 e 5:

  <b></b>

    # Playbook example
      - name: Extract zip file
        win_dsc:
          resource_name: archive
          ensure: Present
          path: "C:\\Temp\\zipfile.zip"
          destination: "C:\\Temp\\Temp2"

      - name: Invoke DSC with check mode
        win_dsc:
          resource_name: windowsfeature
          name: telnet-client

  * Módulo Windows [**win_hotfix**](http://docs.ansible.com/ansible/latest/win_hotfix_module.html): Especifica qual hotfix você quer instalar.

  <b></b>

    - name: install hotfix without validating the KB and Identifier
      win_hotfix:
        source: C:\temp\windows8.1-kb3172729-x64_e8003822a7ef4705cbb65623b72fd3cec73fe222.msu
        state: present
      register: hotfix_install

    - win_reboot:
      when: hotfix_install.reboot_required


### Ansible Vault:

  O [**Ansible Vault**](http://docs.ansible.com/ansible/latest/vault.html#vault-ids-and-multiple-vault-passwords) agora permite senhas diferentes para os arquivos criptografados:

      $ ansible-playbook --vault-id dev-password --vault-id @prompt site.yml

### Automação de Rede (Networking):

O Ansible 2.4 também incluiu novos updates especificamente para automação de rede, incluindo 'Declaração de Intenção' (declarative intent). [**Veja mais sobre**](https://www.ansible.com/blog/networking-features-in-ansible-2-4).

O número de plataformas de redes suportadas cresceu para 33 e o total de módulos só de rede agora é 463 (36% dos módulos Ansible).

### Migrando versões anteriores:

Segue o link com os cuidados que devem ser tomados na atualização do ambiente (em Inglês):

* [**Guia de Migração para 2.4**](http://docs.ansible.com/ansible/2.4/porting_guide_2.4.html)

### Referências:

* [**Ansible Project 2.4 now generally available**](https://www.redhat.com/en/blog/ansible-project-24-now-generally-available)

* [**Changelog completo**](https://github.com/ansible/ansible/blob/stable-2.4/CHANGELOG.md)
