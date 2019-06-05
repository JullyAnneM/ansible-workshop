# Exercício 1 - Comandos Ad hoc
### Testando conectividade com host 
Vamos começar com algo básico - pingar um host. O módulo ping testa a capacidade de resposta do nosso host.

```ansible web -m ping```

### Revisando fatos do host 
O módulo setup exibe fatos de configuração de um host gerenciado pela máquina controle Ansible.

```ansible web -m setup```


