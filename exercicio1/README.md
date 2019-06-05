# Exercício 1 - Comandos Ad hoc

## Introdução
Este diretório contém consigo um arquivo ansible.cfg e um arquivo inventory. Estes devem ser populados com os valores de máquinas criadas em seu ambiente.

## Exercicios 
### Testando conectividade com host 
Vamos começar com algo básico - pingar um host. O módulo ping testa a capacidade de resposta do nosso host.

```ansible all -m ping```

Podemos também pingar apenas um grupo específico, explicitando na chamada o nome. 

```ansible web -m ping```


### Revisando fatos do host 
O módulo setup exibe fatos de configuração de um host gerenciado pela máquina controle Ansible.

```ansible web -m setup```

E para restringir a saída com um valor específico a ser procurado, basta usar um filtro.

```ansible web -m setup -a "filter=ansible_distribution*" ```

### Rodando comando ad hoc
É possível rodar qualquer módulo Ansible desenvolvido pela comunidade como um comando ad hoc.

``` ansible all -m copy -a 'content="Seja bem-vindo ao servidor ({{ ansible_hostname }})" dest=/etc/motd' ```

Pode-se observar que o comando acima descreve exatamente como o módulo copy se comportaria em um Playbook. Para maiores informações sobre o módulo e seus atributos, basta usar a ansible-doc.

```ansible-doc copy```

Caso não esteja claro o exato nome do módulo, também é possível incorporar um padrão específico a pesquisa. 

```ansible-doc -l | grep -i nome_procurado```



