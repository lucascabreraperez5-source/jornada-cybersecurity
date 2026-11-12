# Fundamentos de SIEM — Centralização e análise de eventos de segurança

**Portfólio 06 — SIEM | Cyber Security 101 — TryHackMe (conteúdo relacionado a Security Solutions)**

## Objetivo

Compreender como uma solução SIEM (*Security Information and Event Management*) reúne registros de diferentes sistemas e auxilia a equipe de segurança a investigar atividades suspeitas.

## O que é um SIEM?

Um SIEM centraliza registros de sistemas operacionais, servidores, aplicações e equipamentos de rede. Em vez de consultar cada máquina separadamente, o analista pode pesquisar eventos, comparar horários, relacionar atividades e identificar padrões que merecem investigação.

Um SIEM **não comprova sozinho que ocorreu um ataque**: ele fornece dados e alertas que precisam ser interpretados em seu contexto.

## Conceitos principais

- **Log:** registro de uma atividade, como autenticação, criação de processo ou conexão de rede.
- **Fonte de dados:** sistema ou aplicação que gerou o evento.
- **Ingestão:** envio e armazenamento dos registros na plataforma.
- **Correlação:** relacionamento entre eventos por campos como horário, computador, usuário, IP e identificador de processo.
- **Alerta:** indicação de que determinada regra ou condição foi detectada; precisa de triagem para determinar sua relevância.
- **Falso positivo:** alerta que parece indicar um problema, mas corresponde a uma atividade não maliciosa.

## Conhecendo o Splunk

No laboratório do TryHackMe, utilizei o **Splunk Enterprise**, que permite consultar eventos por meio da linguagem **SPL (Search Processing Language)**.

Alguns recursos importantes:

| Recurso | Para que serve |
|---|---|
| `index` | Seleciona um conjunto de eventos armazenados. |
| `sourcetype` | Identifica o formato ou tipo dos registros. |
| `stats count` | Conta eventos e permite agrupar resultados. |
| `table` | Organiza campos escolhidos em uma tabela. |
| Filtro de tempo | Define o período da investigação. |

Exemplo de consulta para conhecer as fontes disponíveis:

```spl
index=* | stats count by index, sourcetype
```

No laboratório, encontrei registros de **Windows, Linux e servidor web**, demonstrando a centralização de diferentes fontes.

## Eventos Windows e Sysmon

O **Sysmon** registra atividades detalhadas do Windows. Alguns eventos relevantes para uma investigação são:

| Sysmon Event ID | Informação registrada |
|---|---|
| 1 | Criação de processo. |
| 3 | Conexão de rede associada a um processo. |
| 7 | Carregamento de imagem ou módulo. |
| 11 | Criação de arquivo. |

Campos como `Image`, `ParentImage`, `CommandLine`, `DestinationIp`, `DestinationPort` e `ProcessGuid` ajudam a reconstruir o comportamento de um processo.

O `ProcessGuid` é especialmente útil porque relaciona eventos da mesma instância de processo, evitando depender apenas do nome do executável ou do PID.

## Aplicação em um SOC

Uma investigação inicial costuma seguir uma sequência:

1. Entender o alerta e delimitar o período e o equipamento envolvidos.
2. Identificar os processos, usuários e endereços relacionados.
3. Correlacionar eventos de execução, rede e alterações no sistema.
4. Distinguir fatos confirmados de hipóteses e avaliar a necessidade de escalonamento.
5. Registrar as evidências de forma clara para outro analista conseguir revisar a análise.

## Principais aprendizados

Compreendi que o SIEM facilita a visibilidade sobre diferentes sistemas, mas a qualidade da investigação depende dos filtros utilizados, da interpretação dos campos e da correlação entre eventos.

Um executável com nome conhecido pode ter comportamento suspeito, e um comando legítimo pode ser usado de forma indevida. Por isso, **nenhum evento isolado deve ser tratado automaticamente como prova de comprometimento**.

## Atividade prática — Investigação de eventos com Splunk

Para aplicar os conceitos, analisei registros do Sysmon em um laboratório do TryHackMe. Investiguei a execução de um processo, uma conexão de rede, comandos de agendamento de tarefas e a criação de um arquivo DLL.

A análise foi documentada com consultas SPL, resultados reais e capturas do laboratório.

**[Ver atividade prática — Investigação de eventos com Splunk](./pratica-investigacao-siem.md)**

