# Fundamentos de SIEM — Monitoramento e Análise de Eventos de Segurança

**Projeto 06 — SIEM | Cyber Security 101 — TryHackMe (Módulo 11: Security Solutions)**

## Sobre o projeto

Este documento reúne os principais conceitos que estudei sobre SIEM (Security Information and Event Management), uma tecnologia utilizada para centralizar, monitorar e analisar eventos de segurança de diferentes sistemas.

O objetivo é compreender como um SOC utiliza essas ferramentas para identificar atividades suspeitas, investigar alertas e relacionar informações de computadores, servidores e dispositivos de rede.

Como parte dos estudos, utilizei o **Splunk Enterprise** em um laboratório do TryHackMe, aplicando consultas para investigar eventos registrados pelo Windows Sysmon.

## O que estudei

- Funcionamento e principais componentes de um SIEM.
- Coleta, centralização e normalização de logs.
- Diferença entre eventos, alertas e incidentes.
- Conceitos básicos de correlação de eventos.
- Utilização do Splunk e da linguagem SPL.
- Monitoramento de processos e conexões com Sysmon.
- Investigação de atividades suspeitas em um SOC.

---

## 1. O que é um SIEM?

SIEM significa **Security Information and Event Management**, ou Gerenciamento de Informações e Eventos de Segurança.

É uma solução que permite reunir registros produzidos por diferentes equipamentos e aplicações em um único ambiente.

Entre as fontes de informação estão:

- **Windows:** eventos de autenticação, execução de processos e alterações no sistema.
- **Linux:** registros de autenticação SSH, comandos administrativos e atividades de serviços.
- **Firewalls:** conexões permitidas, bloqueadas e tentativas de acesso.
- **Servidores web:** requisições HTTP, erros e comportamentos incomuns.
- **EDR e antivírus:** detecções de ameaças e atividades suspeitas em endpoints.

A principal vantagem é permitir que o analista investigue atividades que envolvem diferentes sistemas sem precisar consultar cada equipamento separadamente.

### Exemplo prático

Imagine que um usuário apresente várias falhas de autenticação e, logo depois, consiga acessar um servidor e executar um programa desconhecido.

Analisando cada evento separadamente, talvez não seja possível compreender toda a situação.

Com um SIEM, o analista pode correlacionar os registros e identificar uma possível sequência de atividades suspeitas.

**Importante:** o SIEM não determina automaticamente que toda atividade incomum é maliciosa. Os alertas precisam ser avaliados dentro do contexto.

---

## 2. Coleta, armazenamento e correlação de logs

O funcionamento básico de um SIEM pode ser dividido em algumas etapas:

| Etapa | Função |
|---|---|
| Coleta | Receber registros de diferentes sistemas e dispositivos |
| Processamento | Interpretar e organizar os dados recebidos |
| Armazenamento | Manter os eventos disponíveis para pesquisa e investigação |
| Correlação | Relacionar eventos que podem representar uma mesma atividade |
| Detecção | Aplicar regras e identificar comportamentos que merecem atenção |
| Investigação | Permitir que o analista consulte e interprete os registros |

### Correlação de eventos

A correlação é especialmente importante em segurança defensiva.

Por exemplo, os seguintes registros poderiam estar relacionados:

1. Um executável desconhecido é iniciado em um computador.
2. O executável estabelece uma conexão de rede.
3. São executados comandos para criar tarefas agendadas.
4. O processo cria arquivos em diretórios temporários.

Cada ocorrência representa uma informação diferente. Quando analisadas em conjunto, podem indicar uma atividade que merece investigação.

Foi justamente esse tipo de relação que procurei compreender durante a prática com o Splunk.

---

## 3. Diferença entre evento, alerta e incidente

Esses três conceitos são importantes para quem pretende trabalhar em SOC.

| Conceito | Significado | Exemplo |
|---|---|---|
| Evento | Registro de alguma atividade observada | Um processo foi iniciado no Windows |
| Alerta | Notificação gerada por uma regra ou mecanismo de detecção | Executável suspeito iniciando uma conexão incomum |
| Incidente | Ocorrência que compromete ou ameaça a segurança e exige tratamento | Comprometimento confirmado de uma conta |

**Nem todo evento gera um alerta, e nem todo alerta representa um incidente confirmado.**

Um analista SOC Nível 1 precisa avaliar as evidências disponíveis, identificar possíveis falsos positivos e determinar se a situação precisa ser escalonada.

---

## 4. Splunk e linguagem SPL

O **Splunk** é uma plataforma utilizada para pesquisar, monitorar e analisar dados de máquinas, incluindo logs de segurança.

No laboratório do TryHackMe, utilizei o Splunk Enterprise para consultar registros de Windows, Linux e aplicações web.

As pesquisas são feitas com a linguagem **SPL (Search Processing Language)**.

### Conceitos principais

- **Index:** conjunto onde os eventos ficam armazenados e podem ser pesquisados.
- **Sourcetype:** identificação do formato ou tipo dos dados.
- **Host:** equipamento associado ao registro.
- **EventCode:** identificador de um evento, como os eventos do Windows.
- **Field:** informação extraída de um registro, como IP, usuário ou processo.

### Consultas básicas

**Pesquisar os eventos de um índice:**

```spl
index="task4"
```

**Procurar um termo nos registros:**

```spl
index="task4" "SharePoint.exe"
```

**Pesquisar eventos de criação de processo:**

```spl
index="task4" EventCode=1
```

**Organizar resultados em uma tabela:**

```spl
index="task4" EventCode=1
| table _time User Image ParentImage CommandLine
```

O símbolo `|` (pipe) envia os resultados de uma etapa da pesquisa para o próximo comando.

O comando `table` permite selecionar as informações mais importantes para a análise, facilitando a leitura dos registros.

Essas consultas foram utilizadas como base para a investigação prática.

---

## 5. Windows Sysmon — Monitoramento de atividades

O **Sysmon (System Monitor)** é uma ferramenta da Microsoft Sysinternals que registra atividades detalhadas do Windows.

Dependendo de sua configuração, pode registrar informações sobre criação de processos, conexões de rede, carregamento de módulos e criação de arquivos.

Esses registros podem ser enviados para plataformas SIEM e utilizados durante investigações.

### Eventos importantes

| Event ID | Descrição |
|---|---|
| 1 | Process Create — criação de processo |
| 3 | Network Connection — conexão de rede |
| 7 | Image Load — carregamento de imagem ou módulo |
| 11 | File Create — criação de arquivo |

### Campos importantes durante uma investigação

**Image:** caminho do executável relacionado ao evento.

**ParentImage:** executável que iniciou o processo analisado.

**CommandLine:** comando utilizado para iniciar um processo.

**ProcessId:** identificador numérico do processo.

**ProcessGuid:** identificador utilizado pelo Sysmon para correlacionar eventos de uma mesma instância de processo.

**DestinationIp:** endereço IP de destino de uma conexão.

**DestinationPort:** porta utilizada na comunicação.

**TargetFilename:** caminho do arquivo criado em um evento de criação de arquivo.

### Por que o ProcessGuid é importante?

O PID de um processo pode ser reutilizado pelo Windows após seu encerramento.

Já o `ProcessGuid` permite relacionar registros associados a uma determinada instância de processo com maior precisão.

Durante a prática, utilizei esse identificador para reunir eventos de criação de processo, conexão de rede, carregamento de módulo e criação de arquivo.

---

## 6. Identificação de atividades suspeitas

Durante uma investigação, alguns comportamentos podem chamar a atenção de um analista:

- Executáveis iniciados a partir de diretórios temporários.
- Programas com nomes que aparentam pertencer a aplicações conhecidas.
- Conexões de rede para destinos ou portas incomuns.
- Criação de tarefas agendadas para execução automática.
- Processos criando arquivos inesperados.
- Atividades que ocorrem em sequência e apresentam relação entre si.

Entretanto, **nenhum desses comportamentos comprova, isoladamente, a presença de malware**.

É necessário verificar o contexto da execução, a origem do arquivo, as ações realizadas e as demais evidências disponíveis.

### Exemplo: tarefas agendadas

O Windows possui o comando `schtasks`, utilizado para criar, consultar e administrar tarefas.

Esse recurso é legítimo e bastante utilizado na administração de sistemas.

Porém, também pode ser abusado por invasores para executar programas automaticamente e estabelecer mecanismos de persistência.

Por isso, a análise deve considerar qual programa será executado, quem criou a tarefa e em quais condições ela será acionada.

---

## 7. Principais aprendizados

Durante este módulo, compreendi que trabalhar com um SIEM envolve muito mais do que pesquisar palavras dentro de logs.

Os principais pontos que aprendi foram:

- A importância de centralizar registros de diferentes sistemas.
- Como pesquisar eventos utilizando comandos básicos de SPL.
- A diferença entre um evento registrado e um incidente confirmado.
- Como relacionar processos, conexões e arquivos utilizando dados do Sysmon.
- A utilidade de campos como `ParentImage`, `ProcessGuid` e `CommandLine`.
- A importância de construir uma linha do tempo antes de apresentar conclusões.
- A necessidade de distinguir comportamentos suspeitos de evidências suficientes para confirmar uma ameaça.

Também percebi que uma boa investigação depende da qualidade dos registros disponíveis e da interpretação correta de cada evento.

---

## 8. Aplicação em Cibersegurança

O conhecimento sobre SIEM está diretamente relacionado às atividades de um **SOC (Security Operations Center)**.

Entre suas aplicações estão:

- Monitorar eventos de autenticação e atividades de endpoints.
- Investigar alertas de segurança e identificar possíveis falsos positivos.
- Correlacionar registros de diferentes fontes.
- Identificar processos, usuários e conexões envolvidos em atividades suspeitas.
- Reconstruir sequências de eventos.
- Documentar evidências e apoiar o escalonamento de incidentes.

Esses conhecimentos são importantes para minha preparação para atuar como **Analista SOC Nível 1**, especialmente nas atividades de monitoramento, triagem e investigação inicial.

---

## Atividade prática — Investigação de Eventos com Splunk SIEM

Para aplicar os conceitos estudados, realizei uma investigação no laboratório **Log Analysis with SIEM**, disponibilizado pelo TryHackMe.

Utilizando o Splunk Enterprise, analisei registros do Windows Sysmon relacionados à execução do arquivo `SharePoint.exe`, localizado em um diretório temporário.

Durante a investigação, identifiquei uma conexão TCP, comandos de criação de tarefas agendadas e um evento de criação de arquivo DLL.

A partir dessas evidências, reconstruí uma linha do tempo da atividade e avaliei os comportamentos encontrados, diferenciando fatos observados de possíveis hipóteses de comprometimento.

A prática foi documentada com as consultas SPL utilizadas, os resultados encontrados e capturas reais da investigação.

**[Ver atividade prática — Investigação de Eventos com Splunk SIEM](./pratica-investigacao-siem.md)**
