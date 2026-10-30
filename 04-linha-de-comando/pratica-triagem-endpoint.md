# Prática — Mini-triagem de Endpoint Windows com PowerShell

**Módulo 4 — Linha de Comando | Cyber Security 101 — TryHackMe**

## Objetivo

Colocar em prática os conhecimentos de PowerShell adquiridos durante o módulo, realizando uma verificação básica do meu computador.

O objetivo foi consultar processos em execução, identificar conexões de rede e analisar registros de autenticação do Windows, relacionando essas informações com atividades de investigação em um SOC.

## Ambiente e ferramentas

- **Sistema operacional:** Windows 11
- **Ferramenta:** Windows PowerShell 5.1
- **Ambiente analisado:** computador pessoal

## 1. Análise de processos

Comecei utilizando o PowerShell para identificar os processos em execução, organizando os resultados pelo maior tempo acumulado de CPU.

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, Id, CPU
```

Entre os resultados, encontrei processos relacionados ao Google Chrome, Epic Games Launcher e componentes do Windows.

A consulta mostrou os nomes dos processos, seus identificadores (PID) e o tempo acumulado de processamento.

**Evidência 1 — Processos em execução**

<img width="1103" height="467" alt="01-processos" src="https://github.com/user-attachments/assets/05575ef7-22e0-4014-9bf0-fdb6fb0c5d9f" />

**O que observei:** o Chrome aparecia com vários processos diferentes, cada um com seu próprio PID. Também entendi que um processo apresentar um tempo elevado de CPU não significa necessariamente que exista alguma atividade suspeita.

## 2. Análise de conexões TCP

Em seguida, utilizei o PowerShell para consultar as conexões TCP estabelecidas no computador.

```powershell
Get-NetTCPConnection -State Established | Select-Object -First 15 LocalAddress, LocalPort, RemoteAddress, RemotePort, OwningProcess
```

O comando retornou informações sobre endereços IP, portas locais e remotas e os identificadores dos processos responsáveis pelas conexões.

Durante a análise, identifiquei conexões na porta remota 443 associadas ao PID `5400`.

**Evidência 2 — Conexões TCP estabelecidas**

<img width="1432" height="899" alt="02-conexoes-tcp-original" src="https://github.com/user-attachments/assets/aa88b0d2-2a5d-4a9e-a303-8407d0a1368a" />

Para descobrir qual programa estava utilizando essas conexões, executei:

```powershell
Get-Process -Id 5400
```

O resultado mostrou que o PID `5400` pertencia ao processo `chrome`, relacionado ao navegador Google Chrome.

**Evidência 3 — Identificação do processo pelo PID**

<img width="1430" height="350" alt="03-identificacao-processo-original" src="https://github.com/user-attachments/assets/6697e609-c8a2-4cbe-ab89-5c7285d66383" />

**O que observei:** consegui relacionar conexões de rede ao programa responsável por elas. Essa análise me ajudou a entender como o PID pode ser utilizado para investigar quais aplicações estão se comunicando com endereços externos.

## 3. Consulta de eventos de autenticação

Na última etapa, utilizei o PowerShell como administrador para consultar registros de autenticação do Windows.

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624,4625} -MaxEvents 10 | Select-Object TimeCreated, Id
```

A consulta retornou dez eventos recentes com o ID `4624`, relacionado a logons bem-sucedidos.

Depois, pesquisei especificamente por eventos de falha de autenticação (4625):

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 5 | Select-Object TimeCreated, Id
```

O PowerShell informou que não foram encontrados registros correspondentes aos critérios da segunda consulta.

**Evidência 4 — Eventos de autenticação do Windows**

<img width="1439" height="899" alt="04-eventos-autenticacao-original" src="https://github.com/user-attachments/assets/28a0d073-bfeb-46c5-a7a2-a256b489f1ab" />

**O que observei:** consegui consultar registros de segurança diretamente pelo PowerShell e identificar eventos relacionados à autenticação.

Também compreendi que encontrar eventos de logon não significa necessariamente identificar uma ameaça. É preciso analisar informações adicionais, como tipo de logon, origem e contexto da atividade.

## Conclusão

Essa prática me ajudou a entender como o PowerShell pode ser utilizado para realizar verificações básicas de segurança em um sistema Windows.

O ponto mais interessante foi conseguir identificar qual processo estava associado a uma conexão de rede, utilizando o PID para relacionar as informações.

Também tive contato com a consulta de eventos de autenticação diretamente pelo terminal e percebi que, durante uma investigação, não basta executar comandos: é necessário interpretar os resultados e considerar o contexto antes de identificar uma possível ameaça.

Foi uma primeira experiência de triagem de endpoint que contribuiu para meus estudos em Segurança Defensiva e SOC.

Também percebi que uma investigação não consiste apenas em executar comandos, mas em interpretar os resultados e compreender quando determinado comportamento realmente merece atenção.

Foi uma primeira experiência de triagem básica de endpoint, que me permitiu aplicar conceitos relacionados à Segurança Defensiva e ao trabalho de um analista SOC.
