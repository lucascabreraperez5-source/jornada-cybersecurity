# Linha de Comando — Windows (CMD e PowerShell) e Linux (Shell)

## Contexto
No Módulo 4 da trilha Cyber Security 101 (TryHackMe), o foco foi a linha de
comando — a "tela de comandos" onde a gente opera o sistema digitando texto,
sem interface gráfica. Em segurança, boa parte do trabalho (investigação,
administração e automação) acontece por aqui, então dominar o básico é essencial.

## O que eu fiz
Pratiquei três ambientes de linha de comando nas salas do módulo:
- Prompt de Comando do Windows (CMD)
- Windows PowerShell
- Shells do Linux (Bash)

## O que descobri
Os três fazem a mesma ideia — dar ordens ao sistema por texto — mas com
"idiomas" (sintaxes) diferentes:

| O que eu quero fazer | Windows CMD | PowerShell | Linux (Bash) |
|---|---|---|---|
| Listar arquivos | `dir` | `Get-ChildItem` | `ls` |
| Ver conteúdo de um arquivo | `type` | `Get-Content` | `cat` |
| Ver onde estou | `cd` | `Get-Location` | `pwd` |
| Ver config de rede | `ipconfig` | `Get-NetIPConfiguration` | `ip a` |
| Testar conexão | `ping` | `Test-Connection` | `ping` |
| Listar processos | `tasklist` | `Get-Process` | `ps` |

O **PowerShell** é o mais moderno e poderoso no Windows (trabalha com objetos,
não só texto), e é muito usado tanto por quem defende quanto por quem ataca.

## Aplicação em Cibersegurança
A linha de comando é onde o analista realmente trabalha: investigar processos
suspeitos, checar conexões de rede, ler logs e automatizar tarefas. Saber ler e
usar PowerShell e Bash é especialmente importante num SOC, porque muita atividade
maliciosa no Windows passa por PowerShell.

## O que aprendi
Fiquei mais confortável navegando e investigando um sistema sem depender da
interface gráfica. Entendi que CMD, PowerShell e Bash são formas diferentes de
"conversar" com o computador, e que o PowerShell é o mais forte no Windows.
Próximo passo: usar esses comandos em investigações reais (análise de logs e
de processos).
