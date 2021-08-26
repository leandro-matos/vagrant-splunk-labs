
## Criação de Máquinas Virtuais para laboratórios de um ambiente Splunk Clusterizado

O **Vagrant** nos auxiliará a criar e gerenciar as máquinas de uma maneira muito mais simples e rápida do que se precisássemos instalá-las manualmente. A solução atualmente está composta por algumas máquinas que fazem o papel de Search-Head, Indexers e alguns forwarders para encaminhamento dos logs. Basta validar o arquivo de configuração do `Vagrantfile` 

## Pré-requisitos

Para utilizar este repositório você deverá instalar o [Vagrant](https://www.vagrantup.com/) e o [VirtualBox](https://www.virtualbox.org/).

Para clonar o repositório você precisará do [git](https://git-scm.com/), para os usuários do Windows recomendamos [https://gitforwindows.org/](https://gitforwindows.org/).

## Configuração

Clone o repositório em algum diretório da sua máquina e inicie as vms:

```bash
git clone https://github.com/leandro-matos/vagrant-splunk-labs
cd vagrant-splunk-labs
vagrant up
```

As máquinas serão provisionadas, este processo leva alguns minutos e depende da sua velocidade de conexão com a internet.

## Utilização

Todos os comandos devem ser utilizados dentro do diretório clonado.

Para listar as máquinas:

```bash
vagrant status
```

Para entrar em uma máquina:

```bash
vagrant ssh nome_maquina
```

Para iniciar as máquinas:

```bash
vagrant up
```

Para desligar as máquinas:

```bash
vagrant halt
```
