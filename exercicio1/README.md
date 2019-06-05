# Exercício 1 - Comandos ad-hoc e Playbooks

## Introdução
Este diretório contém consigo um arquivo ansible.cfg e um arquivo inventory. Estes devem ser populados com os valores de máquinas criadas em seu ambiente.

## Exercicios 
### Testando conectividade com host 
Vamos começar com algo básico - pingar um host. O módulo *ping* testa a capacidade de resposta do nosso host.

```ansible all -m ping```

Podemos também pingar apenas um grupo específico, explicitando na chamada o nome. 

```ansible web -m ping```

### Revisando fatos do host 
O módulo *setup* exibe fatos de configuração de um host gerenciado pela máquina controle Ansible.

```ansible web -m setup```

E para restringir a saída com um valor específico a ser procurado, basta usar um filtro.

```ansible web -m setup -a "filter=ansible_distribution*" ```

### Rodando comando ad hoc
É possível rodar qualquer módulo Ansible desenvolvido pela comunidade como um comando ad hoc.

Instalando apache:

``` ansible web -m yum -a 'name=httpd state=present' ```

Criando arquivo index.html populado em /var/www/html:
``` ansible web -m copy -a 'content="Seja bem-vindo ao servidor {{ ansible_hostname }}" dest=/var/www/html/index.html' ```

Habilitando apache:
``` ansible web -m service -a 'name=httpd state=started enabled=true' ```

Pode-se observar que os comandos acima descrevem exatamente como o módulos se comportariam em um Playbook. Para maiores informações sobre o módulo e seus atributos, basta usar a *ansible-doc*.

```ansible-doc copy```

Caso não esteja claro o exato nome do módulo, também é possível incorporar um padrão específico a pesquisa. 

```ansible-doc -l | grep -i nome_procurado```

### Mantendo comandos ad hoc persistentes ao escrevê-los em um Playbook
Crie um arquivo main.yml. Neste arquivo deverá ter explicitado o host ou grupo de hosts que serão atingidos. 

```
- name: Playbook for documented configuration of hosts
  hosts: web
  become: yes
```

A partir disso, as tarefas de instalação do pacote apache, criação de index.html populado e serviço habilitado devem ser adicionadas.

``` 
  tasks:
    - name: httpd package is present
      yum:
        name: httpd
        state: present

    - name: correct index.html is present
      copy:
        content: "Seja bem-vindo ao servidor {{ ansible_hostname }}"
        dest: /var/www/html/index.html

    - name: httpd is started
      service:
        name: httpd
        state: started
        enabled: true
``` 

### Testando página html em host 
Para testar se o playbook executou como esperado, basta logar-se ao host alvo da automação e usar o comando *curl* no IP ou hostname da máquina.