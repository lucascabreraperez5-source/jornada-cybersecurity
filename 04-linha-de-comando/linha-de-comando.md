# Linha de Comando — Windows (CMD e PowerShell) e Linux (Shell)

## Sobre o projeto

Este estudo faz parte do Módulo 4 da trilha **Cyber Security 101**, na plataforma TryHackMe.

O objetivo foi conhecer melhor a linha de comando e entender como interagir com diferentes sistemas operacionais por meio de comandos, sem depender exclusivamente de interfaces gráficas.

Esse conhecimento é importante em Cibersegurança, pois muitas atividades de administração, investigação e automação são realizadas diretamente pelo terminal.

## O que estudei

Durante as atividades, explorei três ambientes de linha de comando:

- **Windows CMD:** Prompt de Comando tradicional do Windows.
- **Windows PowerShell:** ambiente mais avançado de linha de comando e automação do Windows.
- **Linux Shell (Bash):** interface utilizada para executar comandos e administrar sistemas Linux.

Pratiquei comandos para navegar entre diretórios, consultar arquivos, verificar configurações de rede e visualizar processos em execução.

## Comandos que conheci e pratiquei

Uma parte interessante desse módulo foi perceber como uma mesma tarefa pode ser realizada de maneiras diferentes dependendo do ambiente utilizado.

| Operação | Windows CMD | PowerShell | Linux (Bash) |
|---|---|---|---|
| Listar arquivos | `dir` | `Get-ChildItem` | `ls` |
| Ler conteúdo de arquivos | `type` | `Get-Content` | `cat` |
| Consultar diretório atual | `cd` | `Get-Location` | `pwd` |
| Consultar configuração de rede | `ipconfig` | `Get-NetIPConfiguration` | `ip a` |
| Testar conectividade | `ping` | `Test-Connection` | `ping` |
| Listar processos | `tasklist` | `Get-Process` | `ps` |

Também comecei a entender melhor as diferenças entre esses ambientes, principalmente o funcionamento do PowerShell, que trabalha com objetos e oferece recursos avançados para administração e automação.

## Principais aprendizados

Esse módulo ampliou minha familiaridade com a linha de comando e mostrou como ela permite acessar informações e executar tarefas de maneira mais direta.

Um dos pontos que mais chamou minha atenção foi o PowerShell. Por ser integrado ao ambiente Windows e oferecer recursos de automação, ele é bastante utilizado tanto na administração de sistemas quanto em atividades de segurança.

Também compreendi que conhecer diferentes ambientes de terminal é importante, já que um profissional de Cibersegurança pode precisar investigar sistemas Windows e Linux durante sua rotina.

## Aplicação em Cibersegurança

A linha de comando está presente em diversas atividades de um analista de segurança, especialmente em ambientes SOC.

Entre suas aplicações estão:

- Consultar processos em execução e identificar atividades suspeitas.
- Verificar conexões e configurações de rede.
- Pesquisar informações em arquivos e registros de eventos.
- Executar comandos para auxiliar na investigação de incidentes.
- Automatizar tarefas repetitivas por meio de scripts.

O conhecimento de PowerShell merece atenção especial na segurança de ambientes Windows, pois a ferramenta também pode ser utilizada em atividades maliciosas. Por isso, entender seu funcionamento é importante tanto para administração quanto para detecção de ameaças.

## Próximos passos

Continuar praticando comandos em PowerShell e Bash, aprofundar os conhecimentos em administração de sistemas e começar a aplicar essas ferramentas em atividades de investigação, principalmente na análise de processos, conexões de rede e logs.
