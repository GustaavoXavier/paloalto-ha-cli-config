# Palo Alto Firewall - Active/Passive HA via CLI

Este projeto demonstra a configuração de alta disponibilidade (HA) ativa/passiva em firewalls Palo Alto Networks, utilizando exclusivamente a CLI.

## 🔧 Cenário
A configuração foi realizada em um laboratório com dois firewalls Palo Alto, configurados em modo ativo/passivo. A comunicação de HA foi estabelecida através das interfaces Ethernet1/4 (HA1) e Ethernet1/5 (HA2).

> 💡 Toda a configuração foi feita via linha de comando (CLI), sem uso do GUI.

## 📌 Etapas Realizadas

1. Entrar no modo de configuração
2. Habilitar HA
3. Configurar interfaces para HA
4. Definir o grupo HA e parâmetros
5. Configurar as interfaces HA1 e HA2
6. Habilitar sincronização de estado
7. Commit da configuração
8. Verificação do status

## 💻 Comandos Utilizados

```
configure
set deviceconfig high-availability enabled yes

set network interface ethernet ethernet1/4 ha
set network interface ethernet ethernet1/4 comment H1-HighAvailability
set network interface ethernet ethernet1/5 ha
set network interface ethernet ethernet1/5 comment H2-HighAvailability

set deviceconfig high-availability group group-id 10
set deviceconfig high-availability group group-id 10 description Active
set deviceconfig high-availability group group-id 10 peer-ip x.x.x.x
set deviceconfig high-availability group group-id 10 peer-ip-backup x.x.x.x
set deviceconfig high-availability group group-id 10 configuration-synchronization enabled yes
set deviceconfig high-availability group group-id 10 mode active-passive
set deviceconfig high-availability group group-id 10 election-option device-priority 10
set deviceconfig high-availability group group-id 10 mode active-passive passive-link-state auto

set deviceconfig high-availability interface ha1 port ethernet1/4
set deviceconfig high-availability interface ha1 ip-address x.x.x.x netmask x.x.x.x
set deviceconfig high-availability interface ha1-backup port management
set deviceconfig high-availability interface ha2 port ethernet1/5
set deviceconfig high-availability interface ha2 ip-address x.x.x.x netmask x.x.x.x

set deviceconfig high-availability group group-id 10 state-synchronization enabled yes
commit

Commit job 55 is in progress. Use Ctrl+C to return to command prompt
...................55%...75%..98%.......................100%
Configuration committed successfully

[edit]
admin@firewall-a(passive)# exit
Exiting configuration mode
admin@firewall-a(passive)> show high-availability all

```



