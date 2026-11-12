# Fundamentos de SIEM — Monitoramento e Análise de Eventos de Segurança

---

## Sobre o projeto

Depois de estudar redes, Linux, Windows e linha de comando, comecei a entender como essas informações são utilizadas em conjunto durante uma investigação de segurança.

Foi nesse momento que entrei no conceito de **SIEM (Security Information and Event Management)**, uma tecnologia que permite centralizar e analisar registros de diferentes sistemas.

O objetivo deste módulo foi compreender como um SOC utiliza esses registros para monitorar ambientes, identificar comportamentos suspeitos e investigar possíveis incidentes.

Também tive meu primeiro contato prático com o **Splunk Enterprise**, utilizando registros do Windows Sysmon em um laboratório do TryHackMe.

## O que estudei

- Funcionamento de um SIEM e sua importância para um SOC.
- Coleta, centralização e análise de logs.
- Diferença entre eventos, alertas e incidentes.
- Correlação de eventos de segurança.
- Conceitos básicos do Splunk e da linguagem SPL.
- Monitoramento de processos, conexões e arquivos com Sysmon.
- Investigação de comportamentos suspeitos em endpoints Windows.

---

## 1. O que é um SIEM?

SIEM significa **Security Information and Event Management**, ou Gerenciamento de Informações e Eventos de Segurança.

Sua função é reunir registros de diferentes sistemas em um ambiente centralizado, permitindo que analistas pesquisem eventos, identifiquem padrões e investiguem atividades suspeitas.

Entre as principais fontes de informação estão:

| Fonte | Informações que podem ser analisadas |
|---|---|
| Windows | Autenticações, processos e alterações no sistema |
| Linux | Logins SSH, comandos administrativos e serviços |
| Firewall | Conexões permitidas e bloqueadas |
| Servidores web | Requisições HTTP, erros e acessos |
| EDR e antivírus | Detecções e comportamentos suspeitos em endpoints |

**O que entendi:** o SIEM não substitui o conhecimento sobre sistemas operacionais e redes. Ele reúne informações dessas diferentes áreas e facilita a investigação.

Por exemplo, uma tentativa de autenticação malsucedida, seguida de execução de um processo desconhecido e uma conexão de rede incomum, pode ser mais relevante quando os eventos são analisados em conjunto.

---

## 2. Como funciona a análise de eventos

O funcionamento de um SIEM envolve diferentes etapas:

| Etapa | Função |
|---|---|
| Coleta | Receber registros de diferentes dispositivos e sistemas |
| Processamento | Interpretar e organizar os eventos recebidos |
| Armazenamento | Manter os registros disponíveis para consulta |
| Correlação | Relacionar eventos que podem fazer parte de uma mesma atividade |
| Detecção | Identificar comportamentos definidos como suspeitos |
| Investigação | Permitir que o analista consulte e interprete os resultados |

### Correlação de eventos

A correlação foi um dos conceitos que considerei mais importantes neste módulo.

Durante uma investigação, um único evento pode não ser suficiente para determinar se uma atividade representa uma ameaça.

Por exemplo:

1. Um programa é executado em um computador.
2. Esse programa estabelece uma conexão de rede.
3. São executados comandos para configurar tarefas agendadas.
4. O mesmo processo cria um arquivo em um diretório temporário.

Analisados separadamente, esses registros podem parecer apenas atividades do sistema.

Entretanto, quando relacionados pelo horário, processo e outros identificadores, podem formar uma sequência que merece investigação.

Esse foi o principal conceito que apliquei na atividade prática com o Splunk.

---

## 3. Diferença entre evento, alerta e incidente

Esses três conceitos aparecem constantemente no trabalho de um SOC.

| Conceito | Significado | Exemplo |
|---|---|---|
| Evento | Registro de uma atividade | Um processo foi iniciado |
| Alerta | Notificação de uma possível atividade suspeita | Um executável desconhecido realizou uma conexão incomum |
| Incidente | Ocorrência que compromete ou ameaça a segurança e exige tratamento | Comprometimento confirmado de uma conta |

**Nem todo evento gera um alerta, e nem todo alerta representa um incidente confirmado.**

Um analista SOC precisa verificar as evidências disponíveis antes de determinar se o alerta é um falso positivo, uma atividade legítima ou uma situação que precisa ser investigada ou escalonada.

Isso também significa que encontrar um comportamento incomum não é suficiente para afirmar que houve uma invasão.

---

## 4. Splunk e linguagem SPL

O **Splunk** é uma plataforma utilizada para coletar, pesquisar e analisar dados, incluindo registros de segurança.

No laboratório do TryHackMe, utilizei o Splunk Enterprise para consultar eventos de Windows, Linux e aplicações web.

As pesquisas são realizadas por meio da linguagem **SPL (Search Processing Language)**.

### Conceitos principais

| Campo | Significado |
|---|---|
| `index` | Conjunto onde os eventos estão armazenados |
| `sourcetype` | Tipo ou formato dos registros |
| `host` | Equipamento associado ao evento |
| `EventCode` | Identificador do evento registrado |
| `_time` | Horário associado ao evento |
| `Image` | Caminho do executável relacionado ao registro |

### Consultas básicas que utilizei

**Pesquisar todos os registros de um índice:**

```spl
index="task4"
```

**Procurar referências a um executável:**

```spl
index="task4" "SharePoint.exe"
```

**Filtrar eventos de criação de processos:**

```spl
index="task4" EventCode=1
```

**Organizar os resultados em uma tabela:**

```spl
index="task4" EventCode=1
| table _time User Image ParentImage CommandLine
```

O símbolo `|` (pipe) permite encaminhar o resultado da pesquisa para outro comando.

Já o `table` organiza os campos selecionados em colunas, facilitando a análise.

**O que aprendi:** em um SIEM, não basta encontrar um registro. É importante saber filtrar informações e selecionar os campos que ajudam a compreender o que aconteceu.

---

## 5. Windows Sysmon — Monitoramento de processos e conexões

O **Sysmon (System Monitor)** é uma ferramenta da Microsoft Sysinternals que registra atividades detalhadas do Windows.

Dependendo da configuração, ele pode registrar processos iniciados, conexões de rede, carregamento de módulos e criação de arquivos.

Esses registros são importantes porque ajudam a reconstruir o comportamento de um programa dentro do sistema.

### Event IDs importantes

| Event ID | Evento | Informação registrada |
|---|---|---|
| 1 | Process Create | Criação de um processo |
| 3 | Network Connection | Conexão de rede associada a um processo |
| 7 | Image Load | Carregamento de um módulo ou imagem |
| 11 | File Create | Criação de um arquivo |

### Campos utilizados na investigação

- **Image:** caminho do executável.
- **ParentImage:** processo que iniciou o executável.
- **CommandLine:** comando utilizado para iniciar um processo.
- **ProcessId:** identificador numérico do processo.
- **ProcessGuid:** identificador que permite correlacionar eventos de uma mesma instância de processo.
- **DestinationIp:** endereço IP de destino.
- **DestinationPort:** porta utilizada na comunicação.
- **TargetFilename:** arquivo criado pelo processo.

### Por que o ProcessGuid é importante?

O Windows pode reutilizar o PID de um processo depois que ele é encerrado.

Já o `ProcessGuid` permite identificar de maneira mais precisa uma determinada instância de processo dentro dos registros do Sysmon.

Na atividade prática, utilizei esse campo para relacionar eventos de execução, conexão de rede, carregamento de módulo e criação de arquivo.

Essa foi uma das partes mais interessantes do exercício, porque consegui acompanhar diferentes atividades associadas ao mesmo executável.

---

## 6. Identificação de comportamentos suspeitos

Durante o estudo, percebi que determinados comportamentos podem chamar a atenção de um analista SOC:

- Executáveis iniciados a partir de diretórios temporários.
- Programas utilizando nomes semelhantes aos de aplicações conhecidas.
- Conexões para endereços ou portas incomuns.
- Criação de tarefas agendadas para execução automática.
- Criação de arquivos inesperados.
- Vários eventos relacionados acontecendo em um intervalo curto.

Entretanto, esses comportamentos precisam ser interpretados dentro do contexto.

Por exemplo, o comando `schtasks.exe` é uma ferramenta legítima do Windows, utilizada para administrar tarefas agendadas.

Mas ele também pode ser utilizado indevidamente para configurar a execução automática de programas.

**O ponto principal:** uma ferramenta legítima não garante que seu uso seja legítimo. É necessário analisar quem a executou, qual comando foi utilizado e qual programa estava envolvido.

Da mesma forma, um executável localizado em uma pasta temporária não é automaticamente malicioso, mas pode justificar verificações adicionais.

---

## 7. O que aprendi

Este módulo me ajudou a conectar conhecimentos adquiridos anteriormente em Windows, redes e linha de comando.

Antes, eu analisava processos, conexões e registros de autenticação separadamente. Com o SIEM, comecei a entender como essas informações podem ser reunidas para investigar uma mesma atividade.

Os principais aprendizados foram:

- Pesquisar e filtrar eventos utilizando SPL.
- Identificar os campos mais importantes dos registros.
- Reconhecer eventos do Windows Sysmon.
- Relacionar processos, conexões e arquivos.
- Organizar uma sequência de acontecimentos por data e horário.
- Diferenciar uma evidência observada de uma hipótese de atividade maliciosa.

Também entendi que uma investigação não depende apenas da quantidade de eventos encontrados, mas da capacidade de interpretar e relacionar as informações disponíveis.

---

## 8. Aplicação em Cibersegurança

O conhecimento sobre SIEM está diretamente relacionado ao trabalho de um **SOC (Security Operations Center)**.

No dia a dia, um analista pode utilizar essas ferramentas para:

- Monitorar atividades de usuários e dispositivos.
- Investigar alertas de segurança.
- Correlacionar logs de diferentes fontes.
- Identificar comportamentos suspeitos.
- Reconstruir linhas do tempo de possíveis incidentes.
- Documentar evidências e apoiar o escalonamento de ocorrências.

Para minha preparação como **Analista SOC Júnior**, considero esse um dos conhecimentos mais importantes até agora, principalmente por permitir aplicar conceitos que estudei nos módulos anteriores.

---

## Atividade prática — Investigação de Eventos com Splunk SIEM

Para complementar os conceitos estudados, realizei uma investigação no laboratório **Log Analysis with SIEM**, disponibilizado pelo TryHackMe.

Utilizando o Splunk Enterprise, analisei eventos do Windows Sysmon relacionados à execução de um arquivo chamado `SharePoint.exe`, localizado em um diretório temporário.

Durante a atividade, identifiquei a criação do processo, uma conexão TCP, comandos relacionados à configuração de tarefas agendadas e a criação de um arquivo DLL.

Também utilizei o `ProcessGuid` para relacionar diferentes eventos associados à mesma instância do executável.

A partir dessas informações, reconstruí uma linha do tempo e avaliei os comportamentos encontrados, diferenciando evidências observadas de possíveis hipóteses de comprometimento.

Documentei os comandos SPL utilizados, os principais resultados e as capturas reais da investigação.

**[Ver atividade prática — Investigação de Eventos com Splunk SIEM](./pratica-investigacao-siem.md)**
