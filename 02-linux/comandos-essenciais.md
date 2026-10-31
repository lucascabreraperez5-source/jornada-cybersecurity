# Fundamentos do Linux — Comandos Essenciais e Leitura de Logs

## Sobre o projeto

Este registro faz parte da minha jornada de aprendizado em Cibersegurança, pela trilha **Cyber Security 101**, na plataforma TryHackMe.

Neste módulo, meu foco foi desenvolver familiaridade com o Linux, entender como utilizar o terminal e aprender a trabalhar com arquivos e informações diretamente pela linha de comando.

## O que pratiquei

Ao longo das salas *Linux Fundamentals Part 1, 2 e 3*, pratiquei tarefas essenciais para a administração e análise de sistemas Linux, incluindo:

- Navegação pelo sistema de arquivos e localização de diretórios.
- Listagem, leitura e manipulação de arquivos.
- Compreensão das permissões de acesso e execução.
- Leitura de arquivos de log e busca por informações específicas no terminal.

## Comandos que conheci e pratiquei

| Comando | Para que serve |
|---|---|
| `pwd` | Exibe o diretório atual. |
| `ls` | Lista arquivos e diretórios. |
| `cd` | Permite navegar entre diretórios. |
| `cat` | Exibe o conteúdo de arquivos. |
| `grep` | Pesquisa textos e padrões dentro de arquivos. |
| `chmod` | Altera as permissões de arquivos e diretórios. |

Também estudei o conceito de permissões `rwx`, que representam leitura (*read*), escrita (*write*) e execução (*execute*).

## Principais aprendizados

Esse módulo me mostrou como o terminal Linux permite acessar e analisar informações de forma direta, sem depender de uma interface gráfica.

Um dos pontos mais relevantes foi aprender a consultar arquivos de log e utilizar o `grep` para localizar informações específicas. Essa habilidade é especialmente útil em atividades de segurança, nas quais registros podem ajudar a investigar eventos e identificar comportamentos suspeitos.

## Aplicação em Cibersegurança

O conhecimento de Linux é uma das bases que estou desenvolvendo para seguir na área de **Segurança Defensiva (Blue Team), com foco em SOC Nível 1**.

Meu próximo objetivo é aprofundar a análise de logs, aprender a identificar padrões suspeitos e conectar esses conhecimentos a investigações práticas de segurança.

## Atividade prática — Investigação de Logs de Autenticação SSH

Para complementar os conhecimentos adquiridos neste módulo, realizei uma atividade prática de análise de logs em uma máquina Linux disponibilizada pelo TryHackMe.

Durante o exercício, utilizei comandos como `grep`, `sed`, `sort` e `uniq` para investigar registros de autenticação SSH no arquivo `/var/log/auth.log`.

Identifiquei seis tentativas malsucedidas de autenticação provenientes de um mesmo endereço IP, comparei esses registros com acessos bem-sucedidos e analisei possíveis padrões de tentativa de acesso.

Também aprendi a importância de utilizar filtros adequados para evitar resultados incorretos durante uma investigação.

A atividade foi documentada com os comandos executados, resultados encontrados e capturas reais do terminal.

**[Ver atividade prática — Investigação de Logs SSH no Linux](./pratica-logs-linux.md)**
