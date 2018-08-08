+++
title = "Passo 6 - Playbook"
description = "Primeira Playbook"
weight = 7
+++

## Primeira Playbook

Os comandos Ad-Hoc executam apenas um módulo por vez. Mas e se a gente quiser criar uma sequência de tarefas com os módulos disponíveis? Aí que entram as playbooks.

Utilizando um arquivo no formato YAML (veja [sintaxe YAML](http://docs.ansible.com/ansible/latest/YAMLSyntax.html)), todas as tarefas podem ser definidas em sequência e chamadas através de um único comando `ansible-playbook -i <nome_inventario> <nome_playbook>`.

Uma playbook basicamente é composta por 'play' e 'tasks'. Uma 'play' é análoga a uma jogada ensaiada de basquete. Define-se quais jogadores participarão (Hosts alvo) e quais passos (tasks) serão executados. Num nível básico, uma 'tasks' nada mais é do que uma chamada aos módulos do Ansible.

Veja o exemplo abaixo. **Copie, cole e salve** o arquivo abaixo com o nome **playbook.yml**:

```yaml
---

- name: 1 - Criando servidores web
  hosts: webservers

  tasks:
    # task 1
    - name: garantiar que o Apache esteja na última versão
      package: name=httpd state=latest
    # task 2
    - name: conter a linha '127.0.0.1 teste' no arquivo /etc/hosts
      lineinfile: path=/etc/hosts line='127.0.0.1 teste' state=present

- name: 2 - Criando o servidor de banco
  hosts: dbservers

  tasks:
    # task 1
    - name: garantir que o mysqld (MariaDB) está na última versão
      package: name=mariadb-server state=latest
    # task 2
    - name: garantir que o  mysqld (MariaDB) está executando (e habilitá-lo no boot)
      service: name=mariadb state=started enabled=yes
```

Execute o comando abaixo:

{{% expand "Clique aqui para expandir a saída" %}}

```bash
$ sudo ansible-playbook -i /etc/ansible/hosts-test playbook.yml

PLAY [1 - Criando servidores web] ***************************************************************************

TASK [Gathering Facts] **************************************************************************************
ok: [web1.example.com]
ok: [web2.example.com]

TASK [garantiar que o Apache esteja na última versão] *******************************************************
fatal: [web1.example.com]: FAILED! => {"changed": false, "cmd": "dnf install -y httpd", "failed": "Waiting for process with pid 7653 to finish.", "Running transaction check", "Waiting for process with pid 7653 to finish.", "The downloaded packages were saved in cache until the next successful transaction.", "You can remove cached packages by executing 'dnf clean packages'."]}
changed: [web2.example.com]

TASK [conter a linha '127.0.0.1 teste' no arquivo /etc/hosts] ***********************************************
changed: [web2.example.com]

PLAY [2 - Criando o servidor de banco] **********************************************************************

TASK [Gathering Facts] **************************************************************************************
ok: [db.example.com]

TASK [garantir que o mysqld (MariaDB) está na última versão] ************************************************
changed: [db.example.com]

TASK [garantir que o  mysqld (MariaDB) está executando (e habilitá-lo no boot)] *****************************
changed: [db.example.com]
	to retry, use: --limit @/home/vagrant/playbook.retry

PLAY RECAP **************************************************************************************************
db.example.com             : ok=3    changed=2    unreachable=0    failed=0
web1.example.com           : ok=1    changed=0    unreachable=0    failed=1
web2.example.com           : ok=3    changed=2    unreachable=0    failed=0
```

{{%/expand%}}

{{% alert theme="danger" %}}Como este Guia rápido faz uma simulação de vários Hosts num único servidor, o Ansible tenta executar paralelamente a mesma TASK (package) nos servidores web1 e web2 (no inventário Ansible apontam para 'localhost'). O utilitário yum/dnf não permite a execução paralela para a instalação de pacotes e o erro seguinte ocorreu.{{% /alert %}}


{{% notice warning %}}


fatal: [web1.example.com]: FAILED! => {"changed": false, "cmd": "dnf install -y httpd", "failed": "Waiting for process with pid 7653 to finish."


{{% /notice %}}

{{% alert theme="info" %}}Para maiores informações acerca de Playbook Ansible, [siga essas instruções(Inglês)](http://docs.ansible.com/ansible/latest/playbooks.html){{% /alert %}}
