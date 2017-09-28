+++
title = "Ansible 2.4 — Include x Import"
description = "Veja o como funciona os conceitos de include e import"
weight = 1
+++

### Novo conceito

A nova versão 2.4 do Ansible foi disponibilizada (19/09/2017) na qual foi introduzido o conceito de include e import. Agora é possível dizer se você quer a importação estática ou inclusão dinâmica de tasks e roles.

Primeiramente: o módulo *include* foi descontinuado (continuará disponível até a versão 2.8 do Ansible) e deu lugar para novos módulos: ***import_\**** *(import_tasks, import_role, import_plays)* e ***include_\**** *(include_tasks, include_role).*

### Mas qual a diferença entre import e include?
A diferença básica é o momento em que o Ansible carrega as *tasks* e *roles* para serem executadas: pré-processadas (estática) ou processadas em tempo de execução (dinâmica).
Nativamente o Ansible trabalha com o pré-processamento da playbook para carregar tudo que precisa antes de iniciar a execução. Neste pré-processamento ele já consegue listar as *tasks* e *tags* que foram declaradas. Você pode manipular essas listas através das opções *list-tags, list-tasks* e *start-at-tasks* nos comandos Ansible.
Mas, e se eu quiser trabalhar com loops (***with_\****) para carregar tasks ou roles dinamicamente? Até a versão 2.3 isso não era possível. Na versão 2.4 você utilizará ***include_**** para inclusão dinâmica.

### Estático x Dinâmico:
Se você usar qualquer módulo ***import_\**** *(import_role, import_tasks, etc.)*, ele será estático. Se você utilizar qualquer módulo ***include_\**** *(include_tasks, include_role, etc.)*, ele será dinâmico.

{{% notice tip %}}Para não confundir import e include, utilize o seguinte esquema:

- impor[**t**] = es[**t**]ático

- inclu[**d**]e = [**d**]inâmico
{{%/notice%}}

Resumindo os dois modos de operação:

* Ansible pré-processa todas as importações estáticas (***import_\****) durante o tempo de análise de uma Playbook
* As inclusões dinâmicas (***include_\****) são processadas durante o tempo de execução no ponto ao qual a task ou role é encontrada.

### Restrições e cuidados:

Quando se tratar de opções de tasks Ansible, tais como tags e declarações condicionais (when:)

* Para importações estáticas (***import_\****), as opções da task pai serão copiadas para todas as tasks filhas contidas na importação.

* Para inclusão dinâmica (***include_\****), as opções da task só serão aplicadas a task dinâmica conforme é avaliada, e não será copiada para as tasks filhas

### Vantagens, considerações e limitações:

Usando include x import você tem algumas vantagens, bem como algumas considerações que os usuários devem levar em conta quando escolher qual usar:

A principal vantagem de uso da inclusão dinâmica (***include_\****) são declarações com loop (***with_\****). Quando um loop é usado com ***include_\****, as tasks ou roles incluídas serão executadas uma vez para cada item do loop.

Usando inclusão dinâmica (***include_\****) você tem algumas limitações quando comparadas a declarações ***import_\****:

* Tags que só existem dentro das inclusões dinâmicas (***include_\****), não serão mostradas na saída da opção *--list-tags*.

* Tasks que só existem dentro das inclusões dinâmicas (***include_\****), não serão mostradas na saída da opção *--list-tasks*.

* Você não pode usar notify para disparar um handler que vem dentro de outra inclusão dinâmica (***include_\****). É possível somente se as tasks e handlers tiverem dentro do mesmo ***include_\****.

* Você não pode utilizar a opção *--start-at-task* para iniciar uma execução a partir de uma task dentro de uma inclusão dinâmica, pois elas não foram carregadas durante o pré-processamento do Ansible.

Usando ***import_\**** também pode ter algumas limitações quando comparadas com inclusão dinâmica (***include_\****):

* Como mencionado acima, declarações de loops não podem ser usadas com importação estática (***import_\****).

* Ao usar variáveis para nome de arquivos de destinos ou nomes de role, as variáveis originárias de inventário (hosts/group vars, etc) não podem ser usadas.


### Documentação include\_\* e include\_\*:

Para exemplos de uso, acesse a documentação oficial:

* [include_role] (http://docs.ansible.com/ansible/latest/include_role_module.html)

* [include_tasks] (http://docs.ansible.com/ansible/latest/include_tasks_module.html)

* [import_role] (http://docs.ansible.com/ansible/latest/import_role_module.html)

* [import_tasks] (http://docs.ansible.com/ansible/latest/import_tasks_module.html)

* [import_plays] (http://docs.ansible.com/ansible/latest/import_plays_module.html)

### Fontes:

* [Playbooks Reuse Includes](http://docs.ansible.com/ansible/latest/playbooks_reuse_includes.html)

* [Playbook Reuse](http://docs.ansible.com/ansible/latest/playbooks_reuse.html)



{{%children style="h2" description="true"%}}
