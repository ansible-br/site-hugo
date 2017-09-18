+++
title = "Passo 4 - Módulos"
description = "Checando Os Módulos Disponíveis"
weight = 5
+++
## Checando os módulos disponíveis:
O Ansible vem com um utilitário chamado `ansible-doc`. Nele é possivel consultar todos os módulos disponíveis numa instalação padrão do Ansible:

	$ ansible-doc -l

**Resultado parcial:**

	expect                             Executes a command and responds to prompts
	facter                             Runs the discovery program `facter' on the remote system
	fail                               Fail with custom message
	fetch                              Fetches a file from remote nodes
	file                               Sets attributes of files
	filesystem                         Makes file system on block device
	find                               return a list of files based on specific criteria
	firewalld                          Manage arbitrary ports/services with firewalld
	flowadm                            Manage bandwidth resource control and priority for protocols, se...
	flowdock                           Send a message to a flowdock
	foreman                            Manage Foreman Resources
	:

{{% notice tip %}}
O retorno do comando `ansible-doc -l` é mostrado numa interface parecida com os comandos 'vim' ou 'less'. Nele é possível paginar o texto com as teclas de navegação/page up/down, além pesquisar através do caractere '/' + texto + <*enter*>. Após a pesquisa, digitar a tecla 'n' (next) para mostrar a próxima entrada ou 'p' (preview) para mostrar a entrada anterior. Para sair basta digitar a letra 'q'.
{{% /notice%}}

Com o nome do módulo escolhido, é possível ver a documentação do módulo através do comando `$ ansible-doc <nomedomodulo>`:

	$ ansible-doc add_host

**Resultado:**

{{% expand "Clique aqui para expandir:" %}}
	> ADD_HOST    (/usr/lib/python2.7/site-packages/ansible/modules/inventory/add_host.py)

	  Use variables to create new hosts and groups in inventory for use in later plays
	  of the same playbook. Takes variables so you can define the new hosts more fully.

	  * note: This module has a corresponding action plugin.

	Options (= is mandatory):

	- groups
	        The groups to add the hostname to, comma separated.
	        [Default: (null)]
	= name
	        The hostname/ip of the host to add to the inventory, can include a colon
	        and a port number.
	Notes:
	  * This module bypasses the play host loop and only runs once for all the
	        hosts in the play, if you need it to iterate use a with\_ directive.
	EXAMPLES:
	# add host to group 'just_created' with variable foo=42
	- add_host:
	    name: "{{ ip_from_ec2 }}"
	    groups: just_created
	    foo: 42

	# add a host with a non-standard port local to your machines
	- add_host:
	    name: "{{ new_ip }}:{{ new_port }}"

	# add a host alias that we reach through a tunnel (Ansible <= 1.9)				
	- add_host:
	    hostname: "{{ new_ip }}"
	    ansible_ssh_host: "{{ inventory_hostname }}"
	    ansible_ssh_port: "{{ new_port }}"

	# add a host alias that we reach through a tunnel (Ansible >= 2.0)
	- add_host:
	    hostname: "{{ new_ip }}"
	    ansible_host: "{{ inventory_hostname }}"
	    ansible_port: "{{ new_port }}"


	MAINTAINERS: Ansible Core Team, Seth Vidal

	METADATA:
	        Status: ['stableinterface']
	        Supported_by: core
{{%/ expand %}}

{{% notice info %}}
A documentação oficial do Ansible é gerada automaticamente a partir da documentação dos módulos.
O comando `ansible-doc <nomedomodulo>` e a url http://docs.ansible.com/ansible/nomedomodulo_module.html possuem o mesmo conteúdo.{{% /notice%}}

{{% notice tip %}}
A documentação dos módulos fornece todas as informações detalhadas das opções/parâmetros, além de ricos **EXEMPLOS** para facilitar a sua utilização. Esses exemplos são **tasks** que podem ser utilizadas diretamentes em **playbooks**. Para utilizar através de comandos Ad-Hoc (`$ ansible -m modulo`) é necessário acrescentar o argumento '-a "opcao1=xxx opcao2=yyy"'{{% /notice%}}

{{% alert theme="info" %}}Para maiores informações acerca dos módulos Ansible, [siga essas instruções(Inglês)](http://docs.ansible.com/ansible/latest/modules_intro.html){{% /alert %}}
