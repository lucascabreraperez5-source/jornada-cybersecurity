# Prática — Investigação de Eventos de Segurança com Splunk SIEM

**Projeto 06 — SIEM | TryHackMe — Log Analysis with SIEM**

## Objetivo

Utilizar o Splunk para investigar registros de um endpoint Windows, identificar atividades que merecem atenção e relacionar eventos do Sysmon para construir uma linha do tempo.

O objetivo foi praticar uma triagem inicial de SOC, documentando somente os fatos encontrados nos logs e separando as evidências das hipóteses.

## Ambiente

- **Plataforma:** TryHackMe — laboratório *Log Analysis with SIEM*.
- **Ferramenta:** Splunk Enterprise.
- **Índice analisado:** `task4`.
- **Fonte principal:** Windows Sysmon / Operational.
- **Endpoint:** `WIN-105`.
- **Período dos eventos investigados:** 14/08/2025.

## 1. Identificação dos registros disponíveis

Comecei verificando quais índices e tipos de logs estavam disponíveis no Splunk:

```spl
index=* | stats count by index, sourcetype
```

Encontrei **2.238 eventos** distribuídos entre registros Windows, Linux e acessos web. Para esta investigação, selecionei o índice `task4`, com **691 eventos Windows**.

Como os dados eram históricos, mantive o período de pesquisa como **All time**.

## 2. Identificação de um processo suspeito

Ao procurar referências à porta `5678`, encontrei um evento de conexão relacionado ao executável `C:\Windows\Temp\SharePoint.exe`.

Para investigar a criação desse processo, utilizei:

```spl
index="task4" EventCode=1 Image="*SharePoint.exe"
| table _time User Image ParentImage ParentCommandLine CommandLine ProcessId
```

O evento Sysmon **ID 1** mostrou que o programa foi iniciado às **11:10:22**, com PID `1460`, sob a conta `WIN-105\Ben Foster`. O processo pai registrado foi `C:\Windows\explorer.exe`.

A localização em `C:\Windows\Temp` e o nome semelhante ao de um produto conhecido chamaram atenção. Contudo, o caminho e o nome do arquivo, sozinhos, não comprovam que ele seja malicioso.

**Evidência 1 — Criação do processo e identificação do processo pai**

<img width="1353" height="646" alt="01-execucao-sharepoint" src="https://github.com/user-attachments/assets/2e3cbbbb-86cf-4479-9a62-07fe92d0cebe" />

## 3. Comunicação de rede associada ao processo

Para localizar a comunicação observada, utilizei a busca:

```spl
index="task4" 5678
```

O evento Sysmon **ID 3** indicou que o mesmo processo, PID `1460`, iniciou uma conexão **TCP** de `10.10.61.100:49798` para `10.10.114.80:5678`, às **11:10:24** (horário exibido pelo Splunk).

Os IPs pertencem à rede privada do laboratório. O registro permite identificar a tentativa de comunicação, mas não demonstra por si só qual conteúdo foi transmitido ou se houve uma atividade maliciosa.

**Evidência 2 — Conexão TCP do SharePoint.exe**

 <img width="1094" height="715" alt="02-conexao-tcp-5678" src="https://github.com/user-attachments/assets/c7cd8351-436b-47be-a4c9-ee9e3a8f85d6" />

## 4. Investigação de tarefas agendadas

Em seguida, pesquisei referências ao utilitário de agendamento do Windows:

```spl
index="task4" Image="*schtasks.exe"
| table _time EventCode User Image ParentImage ParentCommandLine CommandLine
```

Encontrei **quatro eventos de criação de processo**, incluindo comandos que tentavam criar a tarefa `Office365 Install` para executar `C:\Windows\Temp\SharePoint.exe`.

Entre os comandos encontrados, havia uma configuração para executar o arquivo no logon e outra para agendá-lo às **15:30**. Também encontrei uma execução de `schtasks /query`, utilizada para consultar tarefas existentes.

Três registros mostraram `C:\Windows\System32\cmd.exe` como processo pai do `schtasks.exe`.

Esses comandos são **compatíveis com uma possível tentativa de persistência**, mas os eventos de criação de processo não confirmam que a tarefa tenha sido efetivamente registrada nem executada.

**Evidência 3 — Comandos de agendamento e consulta de tarefas**

<img width="1349" height="617" alt="03-tarefas-agendadas" src="https://github.com/user-attachments/assets/ab2147fe-29e4-4062-968e-2c3d0431bf12" />

## 5. Criação de arquivo e correlação de eventos

Para relacionar os registros à mesma instância do processo, utilizei o `ProcessGuid` observado nos eventos:

```spl
index="task4" ProcessGuid="{c5d2b969-c41e-689d-dc02-000000002101}"
| table _time EventCode Image CommandLine DestinationIp DestinationPort TargetImage
```

A consulta retornou quatro eventos do Sysmon associados ao `SharePoint.exe`: **1 (criação de processo), 7 (carregamento de módulo), 3 (conexão de rede) e 11 (criação de arquivo)**.

Para identificar o arquivo criado, refinei a pesquisa:

```spl
index="task4" EventCode=11 Image="*SharePoint.exe"
| table _time Image TargetFilename
```

O resultado mostrou que, às **11:14:39**, o processo criou o arquivo:

```text
C:\Users\BENFOS~1\AppData\Local\Temp\5\mtbogt.dll
```

O evento **ID 11 comprova a criação do arquivo**, mas não que a DLL tenha sido carregada ou executada. A presença desse registro, somada às demais atividades, reforçou a necessidade de investigar o processo.

**Evidência 4 — Criação da DLL em diretório temporário**

<img width="1357" height="653" alt="04-criacao-dll" src="https://github.com/user-attachments/assets/d64d03e0-4ea0-4210-8d6c-4765339fa2ad" />

## Linha do tempo da investigação

| Horário exibido | Evento observado |
|---|---|
| 11:10:22 | `SharePoint.exe` iniciado pelo `explorer.exe` |
| 11:10:24 | Conexão TCP para `10.10.114.80:5678` |
| 11:13:20 | Comando para criar tarefa no logon |
| 11:14:17 | Nova tentativa de agendamento no logon |
| 11:14:39 | Criação do arquivo `mtbogt.dll` |
| 11:15:09 | Comando para agendamento às 15:30 |
| 11:15:54 | Consulta de tarefas com `schtasks /query` |

Os horários da tabela seguem a apresentação do Splunk; o registro bruto do Sysmon também inclui horários em UTC.

## Conclusão

Essa prática me ajudou a entender como um SIEM permite relacionar eventos que, isoladamente, poderiam parecer desconectados.

Investiguei a execução de um processo no Windows, sua conexão de rede, comandos associados a tarefas agendadas e a criação de uma DLL. Utilizei buscas em SPL e campos como `Image`, `ParentImage`, `ProcessId` e `ProcessGuid` para organizar os resultados.

**Minha classificação inicial foi de atividade suspeita, com indícios de possível persistência e necessidade de escalonamento para investigação adicional.** Não confirmei que o executável fosse malware, que as tarefas tivessem sido criadas com sucesso ou que a DLL tivesse sido executada.

O principal aprendizado foi que uma investigação de SOC exige **correlação de evidências, atenção ao contexto e cuidado para não transformar hipóteses em conclusões sem comprovação**.

