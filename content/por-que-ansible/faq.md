+++
title = "FAQ"
description = "Perguntas e respostas frequentes"
weight = 9
+++

### Ansible é considerado ou não como gerência de configuração?

Existem diversas discussões na internet sobre se o Ansible é considerado uma ferramenta de Gerência de Configuração. A parte teórica da gerência de configuração se baseia na escola de GCONF apresentado por Mark Burgess, criador do CFEngine, onde o mesmo diz que os princípios do GCONF são Idempotência e estado desejado.

Idempotência é a característica de verificar o estado atual do que será modificado. Se já estiver no estado desejado, nenhuma ação será realizada. O Ansible na versão 2.3 possui mais de 1300 módulos que, na maior parte, seguem o princípio da idempotência. Nos casos em que o Ansible não possui um módulo nativo, há disponível um conjunto de “módulos de comando” (shell, raw, telnet, command, script), que precisam de um tratamento para que se tornem idempotentes, através de atributos “args” ou diretivas apropriadas (when, when_changed, when_failed etc).

Quanto ao item convergência, o criador do Ansible, Michael DeHaan, afirmou no [grupo de discussão do Ansible](https://groups.google.com/forum/#!searchin/ansible-project/convergence%7Csort:relevance/ansible-project/WpRblldA2PQ/lYDpFjBXDlsJ):

“Esta coisa de estado desejado de vez em quando é escrita de forma errada como ‘Convergência’… Convergência tipicamente significa rodar o processo 4 ou 5 vezes até conseguir chegar no estado desejado. Isso é terrível se você quer dar somente um apenas um salto… A indústria está um pouco atormentada com a ideia de que coisas simples devem ser falada em termos complexos e o Ansible não é bem assim. O nosso objetivo é simples — falado em inglês claro: Get Stuff done (faça acontecer)”, em tradução livre.

### Como vejo todas as variáveis de inventário definidas para o meu host?

Executando o seguinte comando, você poderá ver as variáveis que você definiu no inventário:

    $ ansible -m debug -a "var=hostvars['hostname']" localhost


### Outras
Acesse a [FAQ Oficial](http://docs.ansible.com/ansible/latest/faq.html)
