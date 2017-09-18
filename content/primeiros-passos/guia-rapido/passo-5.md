+++
title = "Passo 5 - Inventário"
description = "Primeiro Arquivo De Inventário"
weight = 6
+++
## Primeiro arquivo de Inventário:
O Ansible foi projetado para trabalhar com múltiplos sistemas (Servers, VMs, Dispositivos de rede) ao mesmo tempo. Ele trabalha selecionando partes desses sistemas listados num arquivo de inventário, que por padrão fica salvo em `/etc/ansible/hosts`. É possivel também especificar um arquivo diferente usando o parâmetro `-i <path/nomeinventario>` na linha de comando (Ad-Hoc `$ ansible` ou Playbook `$ ansible-playbook`).

O arquivo padrão de inventário já vem com vários exemplos.

{{% expand "Clique aqui para expandir:" %}}
	# Este é um arquivo 'host' padrão de inventário
	#
	# Ele geralmente é encontrado em /etc/ansible/hosts
	#
	#   - Comentários iniciam com caractere '#'
	#   - Linhas em branco são ignoradas
	#   - Grupos de hosts são delimitados por colchetes [nomedogrupo]
	#   - Você pode adiconar hostnames (DNS) ou endereços IPs
	#   - Um hostname/ip pode ser membro de vários grupos

	# Ex 1: Hosts desagrupados, especificados antes de qualquer grupo.

	## green.example.com ansible_host=192.168.0.100
	## blue.example.com ansible_host=blue
	## 192.168.100.1 ansible_connection=local
	## 192.168.100.10

	# Para cada host é possível especificar uma valor diferente
	# para a mesma variável.

	# Ex 2: Um conjunto de hosts pertencem ao grupo  'webservers'

	## [webservers]
	## alpha.example.org
	## beta.example.org
	## 192.168.1.100
	## 192.168.1.110

	# Se você tem vários hosts seguindo um padrão, você pode especifica-los
	# dessa forma:

	## www[001:006].example.com

	# Ex 3: Um conjunto de servidores de banco de dados estão no grupo 'dbservers'

	## [dbservers]
	##
	## db01.intranet.mydomain.net
	## db02.intranet.mydomain.net
	## 10.25.1.56
	## 10.25.1.57

	# Aqui está outro exemplo de faixa de host, desta vez sem começar
	# com 0s:

	## db-[99:101]-node.example.com

	# Ex 4: Grupo 'datacenter' é a união de n grupos.
	## [datacenter:children]
	## webservers
	## dbservers

	# Ex 5: Variaveis de grupos
	## [datacenter:vars]
	## localizacao=brasil
	## estado=df
{{%/ expand %}}

Criaremos um inventário simplificado com vários hosts que apontam para localhost através da variável ansible_connection=local. Com isso teremos mais de um Host no inventário apontando para um único servidor, simulando vários sistemas.

Copie, cole e salve o conteúdo abaixo no arquivo /etc/ansible/hosts-test

	web1.example.com
	web2.example.com
	db.example.com

	[webservers]
	web1.example.com
	web2.example.com

	[dbservers]
	db.example.com

	[datacenter:children]
	webservers
	dbservers

	[datacenter:vars]
	ansible_connection=local

Checando se todos os hosts (all) do inventário estão respondendo:

	$ ansible -i /etc/ansible/hosts-test -m ping all

	web1.example.com | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}
	web2.example.com | SUCCESS => {
			"changed": false,
			"ping": "pong"
	}

	db.example.com | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}

Checando o grupo webservers:

	$ ansible -i /etc/ansible/hosts-test -m ping webservers

	web1.example.com | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}
	web2.example.com | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}


Checando o grupo dbservers:

	$ ansible -i /etc/ansible/hosts-test -m ping dbservers

	db.example.com | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}

Checando o grupo datacenter (união dos grupos webservers e dbservers):

		$ ansible -i /etc/ansible/hosts-test -m ping datacenter

		db.example.com | SUCCESS => {
				"changed": false,
				"ping": "pong"
		}
		web1.example.com | SUCCESS => {
				"changed": false,
				"ping": "pong"
		}
		web2.example.com | SUCCESS => {
				"changed": false,
				"ping": "pong"
		}


{{% notice tip %}}
Na seleção do host do inventário, é possível informar um padrão diferente de host/grupo. Padrões podem ser expressões regulares, caracteres coringas e outros. Exemplo: `ansible *.example.* -m <nome_modulo> -a <argumentos>`. Para outros padrões, [clique aqui (Inglês)](http://docs.ansible.com/ansible/latest/intro_patterns.html){{% /notice%}}

{{% alert theme="info" %}}Para maiores informações acerca de inventário Ansible, [siga essas instruções(Inglês)](http://docs.ansible.com/ansible/latest/intro_inventory.html){{% /alert %}}
