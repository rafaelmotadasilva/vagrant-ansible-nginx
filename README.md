<h1>
    <a href="https://www.dio.me/">
     <img align="center" width="40px" src="https://www.ansible.com/images/project-logos/ansible-core.svg"></a>
    <span> Criando uma role do Nginx no Ansible</span>
</h1>

Repositório desenvolvido para fins educativos.

## Objetivo

Criar uma máquina virtual através de um arquivo do Vagrantfile. Configurar o provisionamento com Ansible e criar uma role do Nginx para realizar as seguintes tarefas:

- Instalar o Nginx.
- Configurar o site (https://viacep.com.br/exemplo/jquery/).

## Vagrantfile

Este é um exemplo simples de `Vagrantfile`, que cria uma máquina virtual e configura o provisionamento com Ansible.

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.network "public_network"
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
```

Se mais de uma interface de rede estiver disponível na máquina host, o Vagrant solicitará que você escolha qual interface a máquina virtual deve fazer a ponte.

## Executando o script

Para criar e configurar a máquina virtual, execute o seguinte comando no terminal:

```
vagrant up
```

Isso criará e iniciará a máquina virtual e aplicará a configuração do Ansible.

## Estrutura do Projeto

Certifique-se de que seu projeto tenha a seguinte estrutura:

```
.
├── Vagrantfile
├── playbook.yml
└── roles
    └── nginx
        ├── tasks
        │   └── main.yml
        └── files
            └── website
                └── cep.html
```

## playbook.yml

Um exemplo básico de um playbook Ansible para usar a role `nginx`:

```
---
- hosts: all
  become: yes
  roles:
    - nginx
```

## roles/nginx/tasks/main.yml

Crie a role `nginx` para realizar as seguintes tarefas:

```
---
- name: Atualizar o servidor
  apt:
    update_cache: yes

- name: Instalar o Nginx
  apt:
    name: nginx
    state: present

- name: Copiar site para o servidor Nginx
  copy:
    src: website/
    dest: /var/www/html/
    owner: www-data
    group: www-data
    mode: '0755'

- name: Iniciar o serviço do Nginx
  service:
    name: nginx
    state: started
    enabled: yes
```
