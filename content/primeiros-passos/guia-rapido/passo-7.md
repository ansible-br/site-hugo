+++
title = "Passo 7 - Dicas e truques"
description = "Outros comandos e parâmetros utéis no Ansible"
weight = 8
+++

### 1) Criar relação de confiança Hosts Linux

Para se conectar aos Hosts Linux, o meio mais prático é criar relação de confiança entre o Ansible Control e os Hosts alvo:

**Gerar uma chave de conexão**

    $ ssh-key-gen

{{% expand "Expandir saída"%}}
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/usuario/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/usuario/.ssh/id_rsa.
    Your public key has been saved in /home/usuario/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:V3HiftgtJiQ6TZA1sP1GBaIu1ueHJs2sFcVykr0KtbM usuario@localhost.localdomain
    The key's randomart image is:
    +---[RSA 2048]----+
    |        o++ +.o  |
    |        .= B =   |
    |        o O X    |
    |       o = & + . |
    |      o S B B = .|
    |     . . X B + . |
    |        . E .    |
    |         = .     |
    |        .        |
    +----[SHA256]-----+
{{%/expand%}}

**Copiar o ID para o Hosts alvo:**

    $ ssh-copy-id <nome_host>

{{% expand "Expandir saída"%}}
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/usuario/.ssh/id_rsa.pub"
    The authenticity of host 'localhost (::1)' can't be established.
    ECDSA key fingerprint is SHA256:bu8qjbMTNPCVu3Ng9T971h6XMh9K9+i+l8VRrLABg6A.
    ECDSA key fingerprint is MD5:d4:37:ed:7d:db:bd:bc:f8:1e:2a:de:7d:5e:f1:d6:3f.
    Are you sure you want to continue connecting (yes/no)? yes
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    usuario@localhost's password:

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'localhost'"
    and check to make sure that only the key(s) you wanted were added.
{{% /expand %}}

### 2) Outras opções de conexão com os Hosts Alvo:

Solicitando a senha do usuário pelo prompt `--ask-pass`

    $ ansible-playbook -i /etc/ansible/hosts-test playbook.yml --ask-pass
    SSH password:
    PLAY [1 - Criando servidores web] ***************************************************************************
    TASK [Gathering Facts] **************************************************************************************
    ok: [web1.example.com]
    ok: [web2.example.com]

{{% expand "Expandir outras opções de conexão" %}}
    Opções de Conexão:
        controle de quem e como se conectar aos hosts
        -k, --ask-pass      pede senha para a conexão
        --private-key=PRIVATE_KEY_FILE, --key-file=PRIVATE_KEY_FILE
                            usa esse arquivo para autenticar a conexão
        -u REMOTE_USER, --user=REMOTE_USER
                            conecta como este usuário (default=None)
        -c CONNECTION, --connection=CONNECTION
                            tipo de conexão a usar (default=smart)
        -T TIMEOUT, --timeout=TIMEOUT
                            sobrescreve o timeout da conexão em segundos
                            (default=10)
        --ssh-common-args=SSH_COMMON_ARGS
                            especifica os argumentos comuns para passar ao sftp/scp/ssh (e.g.
                            ProxyCommand)
        --sftp-extra-args=SFTP_EXTRA_ARGS
                            especifica os argumentos extras para passar somente ao sftp (e.g. -f,
                            -l)
        --scp-extra-args=SCP_EXTRA_ARGS
                            especifica os argumentos extras para passar somente ao scp  (e.g. -l)
        --ssh-extra-args=SSH_EXTRA_ARGS
                            especifica os argumentos extras para passar somente ao ssh  (e.g. -R)
{{% /expand %}}

Solicitando senha para escalar privilégios pelo prompt `--ask-become-pass`

    ansible-playbook -i /etc/ansible/hosts-test playbook.yml --ask-become-pas
    SUDO password[defaults to SSH password]:
    PLAY [1 - Criando servidores web] ***************************************************************************
    TASK [Gathering Facts] **************************************************************************************
    ok: [web1.example.com]
    ok: [web2.example.com]

{{% expand "Expandir outras opções de escalação de privilégios" %}}
    Privilege Escalation Options:
        control how and which user you become as on target hosts

        -s, --sudo          run operations with sudo (nopasswd) (deprecated, use
                            become)
        -U SUDO_USER, --sudo-user=SUDO_USER
                            desired sudo user (default=root) (deprecated, use
                            become)
        -S, --su            run operations with su (deprecated, use become)
        -R SU_USER, --su-user=SU_USER
                            run operations with su as this user (default=root)
                            (deprecated, use become)
        -b, --become        run operations with become (does not imply password
                            prompting)
        --become-method=BECOME_METHOD
                            privilege escalation method to use (default=sudo),
                            valid choices: [ sudo | su | pbrun | pfexec | doas |
                            dzdo | ksu | runas ]
        --become-user=BECOME_USER
                            run operations as this user (default=root)
        --ask-sudo-pass     ask for sudo password (deprecated, use become)
        --ask-su-pass       ask for su password (deprecated, use become)
        -K, --ask-become-pass
                            ask for privilege escalation password

{{%/expand %}}

### 3) Listagem de Hosts e Tasks das Playbooks:

**Listando todos os Hosts das Playbooks: `--list-hosts`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml --list-hosts

{{% expand "Expandir saída" %}}

    playbook: playbook.yml
      play #1 (webservers): 1 - Criando servidores web	TAGS: []
        pattern: [u'webservers']
        hosts (2):
          web2.example.com
          web1.example.com
      play #2 (dbservers): 2 - Criando o servidor de banco	TAGS: []
        pattern: [u'dbservers']
        hosts (1):
          db.example.com

{{% /expand %}}


**Listando todas as TASKS das Playbooks: `--list-tasks`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml --list-tasks

{{% expand "Expandir saída" %}}    
    playbook: playbook.yml
      play #1 (webservers): 1 - Criando servidores web	TAGS: []
        tasks:
          garantiar que o Apache esteja na última versão	TAGS: []
          conter a linha '127.0.0.1 teste' no arquivo /etc/hosts	TAGS: []

      play #2 (dbservers): 2 - Criando o servidor de banco	TAGS: []
        tasks:
          garantir que o mysqld (MariaDB) está na última versão	TAGS: []
          garantir que o  mysqld (MariaDB) está executando (e habilitá-lo no boot)	TAGS: []
{{% /expand %}}

### 4) Checagem de sintaxe

**Checando a sintaxe YAML: `--syntax-check `**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml --syntax-check

{{% expand "Expandir saída" %}}  
    ERROR! Syntax Error while loading YAML.


    The error appears to have been in '/home/usuario/playbook.yml': line 4, column 10, but may
    be elsewhere in the file depending on the exact syntax problem.

    The offending line appears to be:

    - name: 1 - Criando servidores web
        hosts: webservers
             ^ here  
{{% /expand %}}


### 5) Saltos


**Confirmando a execução das TASKs: `--step`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml --step

{{% expand "Expandir saída" %}}  
    PLAY [1 - Criando servidores web] ***************************************************************************
    Perform task: TASK: Gathering Facts (N)o/(y)es/(c)ontinue: y

    Perform task: TASK: Gathering Facts (N)o/(y)es/(c)ontinue: **************************************************

    TASK [Gathering Facts] **************************************************************************************
    ok: [web2.example.com]
    ok: [web1.example.com]
    Perform task: TASK: garantiar que o Apache esteja na última versão (N)o/(y)es/(c)ontinue: y

    Perform task: TASK: garantiar que o Apache esteja na última versão (N)o/(y)es/(c)ontinue: *******************

    TASK [garantiar que o Apache esteja na última versão] *******************************************************
    ok: [web2.example.com]
    ok: [web1.example.com]  
{{% /expand %}}



**Iniciando a Playbook a partir de uma TASK: `--start-at-task`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml --start-at-task="garantir que o mysqld (MariaDB) está na última versão"

{{% expand "Expandir saída" %}}    
    PLAY [1 - Criando servidores web] ***************************************************************************

    PLAY [2 - Criando o servidor de banco] **********************************************************************

    TASK [Gathering Facts] **************************************************************************************
    ok: [db.example.com]

    TASK [garantir que o mysqld (MariaDB) está na última versão] ************************************************
    ok: [db.example.com]

    TASK [garantir que o  mysqld (MariaDB) está executando (e habilitá-lo no boot)] *****************************
    ok: [db.example.com]

    PLAY RECAP **************************************************************************************************
    db.example.com             : ok=3    changed=0    unreachable=0    failed=0
{{% /expand %}}


**Limitando a execução da playbook: `--limit`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml --limit web1.example.com

{{% expand "Expandir saída" %}}   
    PLAY [1 - Criando servidores web] ***************************************************************************

    TASK [Gathering Facts] **************************************************************************************
    ok: [web1.example.com]

    TASK [garantiar que o Apache esteja na última versão] *******************************************************
    ok: [web1.example.com]

    TASK [conter a linha '127.0.0.1 teste' no arquivo /etc/hosts] ***********************************************
    ok: [web1.example.com]

    PLAY [2 - Criando o servidor de banco] **********************************************************************
    skipping: no hosts matched

    PLAY RECAP **************************************************************************************************
    web1.example.com           : ok=3    changed=0    unreachable=0    failed=0

{{% /expand %}}

### 6) Depurando (Debug)
**Execundo a playbook em Debug modo 1: `-v`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml -v

{{% expand "Expandir saída" %}}
    Using /etc/ansible/ansible.cfg as config file

    PLAY [1 - Criando servidores web] ***************************************************************************

    TASK [Gathering Facts] **************************************************************************************
    ok: [web1.example.com]
    ok: [web2.example.com]

    TASK [garantiar que o Apache esteja na última versão] *******************************************************
    ok: [web2.example.com] => {"changed": false, "msg": "Nothing to do"}
    ok: [web1.example.com] => {"changed": false, "msg": "Nothing to do"}
{{% /expand %}}

**Execundo a playbook em Debug modo 2: `-vv`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml -vv

{{% expand "Expandir saída" %}}
    Using /etc/ansible/ansible.cfg as config file

    PLAYBOOK: playbook.yml **************************************************************************************
    2 plays in playbook.yml

    PLAY [1 - Criando servidores web] ***************************************************************************

    TASK [Gathering Facts] **************************************************************************************
    ok: [web1.example.com]
    ok: [web2.example.com]
    META: ran handlers

    TASK [garantiar que o Apache esteja na última versão] *******************************************************
    task path: /home/vagrant/playbook.yml:8
    ok: [web1.example.com] => {"changed": false, "msg": "Nothing to do"}
    ok: [web2.example.com] => {"changed": false, "msg": "Nothing to do"}
{{% /expand %}}


**Executando a playbook em Debug modo 3: `-vvv`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml -vvv

{{% expand "Expandir saída" %}}
    Using /etc/ansible/ansible.cfg as config file

    PLAYBOOK: playbook.yml **************************************************************************************
    2 plays in playbook.yml

    PLAY [1 - Criando servidores web] ***************************************************************************

    TASK [Gathering Facts] **************************************************************************************
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/system/setup.py
    <web2.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web2.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/system/setup.py
    <web1.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web1.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416 `" && echo ansible-tmp-1505758887.22-149098275158416="` echo /root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416 `" ) && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507 `" && echo ansible-tmp-1505758887.23-186920271278507="` echo /root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507 `" ) && sleep 0'
    <web2.example.com> PUT /tmp/tmpr3Pb0C TO /root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416/setup.py
    <web2.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416/ /root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416/setup.py && sleep 0'
    <web1.example.com> PUT /tmp/tmpXsqZud TO /root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507/setup.py
    <web1.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507/ /root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507/setup.py && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507/setup.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505758887.23-186920271278507/" > /dev/null 2>&1 && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416/setup.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505758887.22-149098275158416/" > /dev/null 2>&1 && sleep 0'
    ok: [web2.example.com]
    ok: [web1.example.com]
    META: ran handlers

    TASK [garantiar que o Apache esteja na última versão] *******************************************************
    task path: /home/vagrant/playbook.yml:8

    Using module file /usr/lib/python2.7/site-packages/ansible/modules/packaging/os/dnf.py
    <web2.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web2.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/packaging/os/dnf.py
    <web1.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web1.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402 `" && echo ansible-tmp-1505758887.98-268212739924402="` echo /root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402 `" ) && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879 `" && echo ansible-tmp-1505758887.99-60716236244879="` echo /root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879 `" ) && sleep 0'
    <web1.example.com> PUT /tmp/tmpoO3S1e TO /root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879/dnf.py
    <web1.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879/ /root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879/dnf.py && sleep 0'
    <web2.example.com> PUT /tmp/tmprEr4zm TO /root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402/dnf.py
    <web2.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402/ /root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402/dnf.py && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879/dnf.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505758887.99-60716236244879/" > /dev/null 2>&1 && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402/dnf.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505758887.98-268212739924402/" > /dev/null 2>&1 && sleep 0'
    Using /etc/ansible/ansible.cfg as config file
{{% /expand %}}


**Execundo a playbook em Debug de conexão: `-vvvv`**

    $ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml -vvvv

{{% expand "Expandir saída" %}}
    Using /etc/ansible/ansible.cfg as config file
    Loading callback plugin default of type stdout, v2.0 from /usr/lib/python2.7/site-packages/ansible/plugins/callback/__init__.pyc

    PLAYBOOK: playbook.yml ***************************************************************************************************************************************
    2 plays in playbook.yml

    PLAY [1 - Criando servidores web] ****************************************************************************************************************************

    TASK [Gathering Facts] ***************************************************************************************************************************************
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/system/setup.py
    <web2.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web2.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/system/setup.py
    <web1.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web1.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180 `" && echo ansible-tmp-1505759060.8-152711064366180="` echo /root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180 `" ) && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054 `" && echo ansible-tmp-1505759060.81-245677638709054="` echo /root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054 `" ) && sleep 0'
    <web2.example.com> PUT /tmp/tmpaKgICu TO /root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180/setup.py
    <web2.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180/ /root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180/setup.py && sleep 0'
    <web1.example.com> PUT /tmp/tmpffkXRp TO /root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054/setup.py
    <web1.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054/ /root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054/setup.py && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180/setup.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759060.8-152711064366180/" > /dev/null 2>&1 && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054/setup.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759060.81-245677638709054/" > /dev/null 2>&1 && sleep 0'
    ok: [web1.example.com]
    ok: [web2.example.com]
    META: ran handlers

    TASK [garantiar que o Apache esteja na última versão] ********************************************************************************************************
    task path: /home/vagrant/playbook.yml:8
    Running dnf
    Running dnf
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/packaging/os/dnf.py
    <web2.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web2.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/packaging/os/dnf.py
    <web1.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web1.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844 `" && echo ansible-tmp-1505759061.68-97065988608844="` echo /root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844 `" ) && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213 `" && echo ansible-tmp-1505759061.69-112968493935213="` echo /root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213 `" ) && sleep 0'
    <web1.example.com> PUT /tmp/tmp6j5O7C TO /root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213/dnf.py
    <web1.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213/ /root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213/dnf.py && sleep 0'
    <web2.example.com> PUT /tmp/tmpt7DaRm TO /root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844/dnf.py
    <web2.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844/ /root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844/dnf.py && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844/dnf.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759061.68-97065988608844/" > /dev/null 2>&1 && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213/dnf.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759061.69-112968493935213/" > /dev/null 2>&1 && sleep 0'
    ok: [web1.example.com] => {
        "changed": false,
        "invocation": {
            "module_args": {
                "conf_file": null,
                "disable_gpg_check": false,
                "disablerepo": [],
                "enablerepo": [],
                "installroot": "/",
                "list": null,
                "name": [
                    "httpd"
                ],
                "state": "latest"
            }
        },
        "msg": "Nothing to do"
    }
    ok: [web2.example.com] => {
        "changed": false,
        "invocation": {
            "module_args": {
                "conf_file": null,
                "disable_gpg_check": false,
                "disablerepo": [],
                "enablerepo": [],
                "installroot": "/",
                "list": null,
                "name": [
                    "httpd"
                ],
                "state": "latest"
            }
        },
        "msg": "Nothing to do"
    }

    TASK [conter a linha '127.0.0.1 teste' no arquivo /etc/hosts] ************************************************************************************************
    task path: /home/vagrant/playbook.yml:11
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/files/lineinfile.py
    <web2.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web2.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/files/lineinfile.py
    <web1.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <web1.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360 `" && echo ansible-tmp-1505759065.55-162492297075360="` echo /root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360 `" ) && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102 `" && echo ansible-tmp-1505759065.55-181080519971102="` echo /root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102 `" ) && sleep 0'
    <web1.example.com> PUT /tmp/tmp4voCMr TO /root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102/lineinfile.py
    <web1.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102/ /root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102/lineinfile.py && sleep 0'
    <web2.example.com> PUT /tmp/tmpCqHjcI TO /root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360/lineinfile.py
    <web2.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360/ /root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360/lineinfile.py && sleep 0'
    <web1.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102/lineinfile.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759065.55-181080519971102/" > /dev/null 2>&1 && sleep 0'
    <web2.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360/lineinfile.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759065.55-162492297075360/" > /dev/null 2>&1 && sleep 0'
    ok: [web1.example.com] => {
        "backup": "",
        "changed": false,
        "diff": [
            {
                "after": "",
                "after_header": "/etc/hosts (content)",
                "before": "",
                "before_header": "/etc/hosts (content)"
            },
            {
                "after_header": "/etc/hosts (file attributes)",
                "before_header": "/etc/hosts (file attributes)"
            }
        ],
        "invocation": {
            "module_args": {
                "attributes": null,
                "backrefs": false,
                "backup": false,
                "content": null,
                "create": false,
                "delimiter": null,
                "directory_mode": null,
                "follow": false,
                "force": null,
                "group": null,
                "insertafter": null,
                "insertbefore": null,
                "line": "127.0.0.1 teste",
                "mode": null,
                "owner": null,
                "path": "/etc/hosts",
                "regexp": null,
                "remote_src": null,
                "selevel": null,
                "serole": null,
                "setype": null,
                "seuser": null,
                "src": null,
                "state": "present",
                "unsafe_writes": null,
                "validate": null
            }
        },
        "msg": ""
    }
    ok: [web2.example.com] => {
        "backup": "",
        "changed": false,
        "diff": [
            {
                "after": "",
                "after_header": "/etc/hosts (content)",
                "before": "",
                "before_header": "/etc/hosts (content)"
            },
            {
                "after_header": "/etc/hosts (file attributes)",
                "before_header": "/etc/hosts (file attributes)"
            }
        ],
        "invocation": {
            "module_args": {
                "attributes": null,
                "backrefs": false,
                "backup": false,
                "content": null,
                "create": false,
                "delimiter": null,
                "directory_mode": null,
                "follow": false,
                "force": null,
                "group": null,
                "insertafter": null,
                "insertbefore": null,
                "line": "127.0.0.1 teste",
                "mode": null,
                "owner": null,
                "path": "/etc/hosts",
                "regexp": null,
                "remote_src": null,
                "selevel": null,
                "serole": null,
                "setype": null,
                "seuser": null,
                "src": null,
                "state": "present",
                "unsafe_writes": null,
                "validate": null
            }
        },
        "msg": ""
    }
    META: ran handlers
    META: ran handlers

    PLAY [2 - Criando o servidor de banco] ***********************************************************************************************************************

    TASK [Gathering Facts] ***************************************************************************************************************************************
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/system/setup.py
    <db.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <db.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <db.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873 `" && echo ansible-tmp-1505759065.97-109285672908873="` echo /root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873 `" ) && sleep 0'
    <db.example.com> PUT /tmp/tmps2iGBs TO /root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873/setup.py
    <db.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873/ /root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873/setup.py && sleep 0'
    <db.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873/setup.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759065.97-109285672908873/" > /dev/null 2>&1 && sleep 0'
    ok: [db.example.com]
    META: ran handlers

    TASK [garantir que o mysqld (MariaDB) está na última versão] *************************************************************************************************
    task path: /home/vagrant/playbook.yml:19
    Running dnf
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/packaging/os/dnf.py
    <db.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <db.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <db.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914 `" && echo ansible-tmp-1505759066.31-218005801299914="` echo /root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914 `" ) && sleep 0'
    <db.example.com> PUT /tmp/tmpfj3ysn TO /root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914/dnf.py
    <db.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914/ /root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914/dnf.py && sleep 0'
    <db.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914/dnf.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759066.31-218005801299914/" > /dev/null 2>&1 && sleep 0'
    ok: [db.example.com] => {
        "changed": false,
        "invocation": {
            "module_args": {
                "conf_file": null,
                "disable_gpg_check": false,
                "disablerepo": [],
                "enablerepo": [],
                "installroot": "/",
                "list": null,
                "name": [
                    "mariadb-server"
                ],
                "state": "latest"
            }
        },
        "msg": "Nothing to do"
    }

    TASK [garantir que o  mysqld (MariaDB) está executando (e habilitá-lo no boot)] ******************************************************************************
    task path: /home/vagrant/playbook.yml:22
    Running systemd
    Using module file /usr/lib/python2.7/site-packages/ansible/modules/system/systemd.py
    <db.example.com> ESTABLISH LOCAL CONNECTION FOR USER: root
    <db.example.com> EXEC /bin/sh -c 'echo ~ && sleep 0'
    <db.example.com> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273 `" && echo ansible-tmp-1505759067.92-281167143879273="` echo /root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273 `" ) && sleep 0'
    <db.example.com> PUT /tmp/tmpu4HcSx TO /root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273/systemd.py
    <db.example.com> EXEC /bin/sh -c 'chmod u+x /root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273/ /root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273/systemd.py && sleep 0'
    <db.example.com> EXEC /bin/sh -c '/usr/bin/python /root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273/systemd.py; rm -rf "/root/.ansible/tmp/ansible-tmp-1505759067.92-281167143879273/" > /dev/null 2>&1 && sleep 0'
    ok: [db.example.com] => {
        "changed": false,
        "enabled": true,
        "invocation": {
            "module_args": {
                "daemon_reload": false,
                "enabled": true,
                "masked": null,
                "name": "mariadb",
                "no_block": false,
                "state": "started",
                "user": false
            }
        },
        "name": "mariadb",
        "state": "started",
        "status": {
            "ActiveEnterTimestamp": "Mon 2017-09-18 16:15:55 UTC",
            "ActiveEnterTimestampMonotonic": "598708833",
            "ActiveExitTimestampMonotonic": "0",
            "ActiveState": "active",
            "After": "sysinit.target network.target system.slice basic.target -.mount systemd-journald.socket",
            "AllowIsolate": "no",
            "AmbientCapabilities": "0",
            "AssertResult": "yes",
            "AssertTimestamp": "Mon 2017-09-18 16:15:43 UTC",
            "AssertTimestampMonotonic": "587492647",
            "Before": "shutdown.target multi-user.target",
            "BlockIOAccounting": "no",
            "BlockIOWeight": "18446744073709551615",
            "CPUAccounting": "no",
            "CPUQuotaPerSecUSec": "infinity",
            "CPUSchedulingPolicy": "0",
            "CPUSchedulingPriority": "0",
            "CPUSchedulingResetOnFork": "no",
            "CPUShares": "18446744073709551615",
            "CPUUsageNSec": "18446744073709551615",
            "CanIsolate": "no",
            "CanReload": "no",
            "CanStart": "yes",
            "CanStop": "yes",
            "CapabilityBoundingSet": "18446744073709551615",
            "ConditionResult": "yes",
            "ConditionTimestamp": "Mon 2017-09-18 16:15:43 UTC",
            "ConditionTimestampMonotonic": "587492647",
            "Conflicts": "shutdown.target",
            "ControlGroup": "/system.slice/mariadb.service",
            "ControlPID": "0",
            "DefaultDependencies": "yes",
            "Delegate": "no",
            "Description": "MariaDB 10.1 database server",
            "DevicePolicy": "auto",
            "ExecMainCode": "0",
            "ExecMainExitTimestampMonotonic": "0",
            "ExecMainPID": "10637",
            "ExecMainStartTimestamp": "Mon 2017-09-18 16:15:54 UTC",
            "ExecMainStartTimestampMonotonic": "598269557",
            "ExecMainStatus": "0",
            "ExecStart": "{ path=/usr/libexec/mysqld ; argv[]=/usr/libexec/mysqld --basedir=/usr $MYSQLD_OPTS $_WSREP_NEW_CLUSTER ; ignore_errors=no ; start_time=[Mon 2017-09-18 16:15:54 UTC] ; stop_time=[n/a] ; pid=10637 ; code=(null) ; status=0/0 }",
            "ExecStartPost": "{ path=/usr/libexec/mysql-check-upgrade ; argv[]=/usr/libexec/mysql-check-upgrade ; ignore_errors=no ; start_time=[Mon 2017-09-18 16:15:54 UTC] ; stop_time=[Mon 2017-09-18 16:15:55 UTC] ; pid=10667 ; code=exited ; status=0 }",
            "ExecStartPre": "{ path=/usr/libexec/mysql-prepare-db-dir ; argv[]=/usr/libexec/mysql-prepare-db-dir %n ; ignore_errors=no ; start_time=[Mon 2017-09-18 16:15:43 UTC] ; stop_time=[Mon 2017-09-18 16:15:54 UTC] ; pid=10477 ; code=exited ; status=0 }",
            "ExecStopPost": "{ path=/usr/libexec/mysql-wait-stop ; argv[]=/usr/libexec/mysql-wait-stop ; ignore_errors=no ; start_time=[n/a] ; stop_time=[n/a] ; pid=0 ; code=(null) ; status=0/0 }",
            "FailureAction": "none",
            "FileDescriptorStoreMax": "0",
            "FragmentPath": "/usr/lib/systemd/system/mariadb.service",
            "Group": "mysql",
            "GuessMainPID": "yes",
            "IOScheduling": "0",
            "Id": "mariadb.service",
            "IgnoreOnIsolate": "no",
            "IgnoreSIGPIPE": "yes",
            "InactiveEnterTimestampMonotonic": "0",
            "InactiveExitTimestamp": "Mon 2017-09-18 16:15:43 UTC",
            "InactiveExitTimestampMonotonic": "587493777",
            "JobTimeoutAction": "none",
            "JobTimeoutUSec": "infinity",
            "KillMode": "process",
            "KillSignal": "15",
            "LimitAS": "18446744073709551615",
            "LimitASSoft": "18446744073709551615",
            "LimitCORE": "18446744073709551615",
            "LimitCORESoft": "18446744073709551615",
            "LimitCPU": "18446744073709551615",
            "LimitCPUSoft": "18446744073709551615",
            "LimitDATA": "18446744073709551615",
            "LimitDATASoft": "18446744073709551615",
            "LimitFSIZE": "18446744073709551615",
            "LimitFSIZESoft": "18446744073709551615",
            "LimitLOCKS": "18446744073709551615",
            "LimitLOCKSSoft": "18446744073709551615",
            "LimitMEMLOCK": "65536",
            "LimitMEMLOCKSoft": "65536",
            "LimitMSGQUEUE": "819200",
            "LimitMSGQUEUESoft": "819200",
            "LimitNICE": "0",
            "LimitNICESoft": "0",
            "LimitNOFILE": "4096",
            "LimitNOFILESoft": "1024",
            "LimitNPROC": "1880",
            "LimitNPROCSoft": "1880",
            "LimitRSS": "18446744073709551615",
            "LimitRSSSoft": "18446744073709551615",
            "LimitRTPRIO": "0",
            "LimitRTPRIOSoft": "0",
            "LimitRTTIME": "18446744073709551615",
            "LimitRTTIMESoft": "18446744073709551615",
            "LimitSIGPENDING": "1880",
            "LimitSIGPENDINGSoft": "1880",
            "LimitSTACK": "18446744073709551615",
            "LimitSTACKSoft": "8388608",
            "LoadState": "loaded",
            "MainPID": "10637",
            "MemoryAccounting": "no",
            "MemoryCurrent": "18446744073709551615",
            "MemoryLimit": "18446744073709551615",
            "MountFlags": "0",
            "NFileDescriptorStore": "0",
            "Names": "mariadb.service",
            "NeedDaemonReload": "no",
            "Nice": "0",
            "NoNewPrivileges": "no",
            "NonBlocking": "no",
            "NotifyAccess": "main",
            "OOMScoreAdjust": "0",
            "OnFailureJobMode": "replace",
            "PermissionsStartOnly": "no",
            "PrivateDevices": "no",
            "PrivateNetwork": "no",
            "PrivateTmp": "yes",
            "ProtectHome": "no",
            "ProtectSystem": "no",
            "RefuseManualStart": "no",
            "RefuseManualStop": "no",
            "RemainAfterExit": "no",
            "Requires": "sysinit.target -.mount system.slice",
            "RequiresMountsFor": "/tmp /var/tmp",
            "Restart": "on-abort",
            "RestartUSec": "5s",
            "Result": "success",
            "RootDirectoryStartOnly": "no",
            "RuntimeDirectoryMode": "0755",
            "RuntimeMaxUSec": "infinity",
            "SameProcessGroup": "no",
            "SecureBits": "0",
            "SendSIGHUP": "no",
            "SendSIGKILL": "no",
            "Slice": "system.slice",
            "StandardError": "inherit",
            "StandardInput": "null",
            "StandardOutput": "journal",
            "StartLimitAction": "none",
            "StartLimitBurst": "5",
            "StartLimitInterval": "10000000",
            "StartupBlockIOWeight": "18446744073709551615",
            "StartupCPUShares": "18446744073709551615",
            "StateChangeTimestamp": "Mon 2017-09-18 16:15:55 UTC",
            "StateChangeTimestampMonotonic": "598708833",
            "StatusErrno": "0",
            "StatusText": "Taking your SQL requests now...",
            "StopWhenUnneeded": "no",
            "SubState": "running",
            "SyslogFacility": "3",
            "SyslogLevel": "6",
            "SyslogLevelPrefix": "yes",
            "SyslogPriority": "30",
            "SystemCallErrorNumber": "0",
            "TTYReset": "no",
            "TTYVHangup": "no",
            "TTYVTDisallocate": "no",
            "TasksAccounting": "yes",
            "TasksCurrent": "26",
            "TasksMax": "512",
            "TimeoutStartUSec": "5min",
            "TimeoutStopUSec": "5min",
            "TimerSlackNSec": "50000",
            "Transient": "no",
            "Type": "notify",
            "UMask": "0007",
            "UnitFilePreset": "disabled",
            "UnitFileState": "enabled",
            "User": "mysql",
            "UtmpMode": "init",
            "WantedBy": "multi-user.target",
            "WatchdogTimestamp": "Mon 2017-09-18 16:15:54 UTC",
            "WatchdogTimestampMonotonic": "598648636",
            "WatchdogUSec": "0"
        }
    }
    META: ran handlers
    META: ran handlers

    PLAY RECAP ***************************************************************************************************************************************************
    db.example.com             : ok=3    changed=0    unreachable=0    failed=0
    web1.example.com           : ok=3    changed=0    unreachable=0    failed=0
    web2.example.com           : ok=3    changed=0    unreachable=0    failed=0
{{% /expand %}}
