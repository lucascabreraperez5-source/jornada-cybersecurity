# Prática — Triagem de Alertas em um SOC

> **Segurança Defensiva | Laboratório SOC L1 Alert Triage — TryHackMe**

## Sobre a prática

Nesta atividade, utilizei um painel SOC simulado do TryHackMe para acompanhar o processo de triagem de alertas. O objetivo foi trabalhar a priorização, atribuição, classificação e documentação de três casos de segurança.

A investigação foi realizada com as informações fornecidas pelo próprio laboratório. As classificações descritas abaixo foram confirmadas como corretas pela plataforma, mas não representam uma investigação forense completa em um ambiente real.

## Ambiente utilizado

- TryHackMe — **SOC L1 Alert Triage**.
- Painel **TryHackMe SIEM**, disponibilizado no navegador.
- Alertas simulados com campos de severidade, usuário, host, status e veredito.

## 1. Reconhecimento e priorização

O painel mostrava cinco alertas: três aguardando atendimento e dois já encerrados. Os três pendentes tinham severidades **Critical**, **High** e **Low**.

Segui a ordem de prioridade apresentada no exercício, começando pelo alerta crítico. Em cada caso, atribuí o atendimento a `You (L1)`, alterei o status para `In Progress`, examinei os detalhes e registrei a decisão de triagem.

## 2. Alerta crítico — Potential Data Exfiltration

O primeiro alerta foi disparado porque um equipamento enviou **5,8 GB** de dados a um mesmo destino, ultrapassando o limite de 5 GB definido pela regra.

| Campo | Valor |
|---|---|
| Severidade | Critical |
| IP de origem | `192.168.45.66` |
| Rede de origem | `UK04/MEETINGROOM` |
| Destino | `*.zoom.us` |
| Dados enviados | 5,8 GB |
| Dados recebidos | 5,2 GB |

<img width="1270" height="566" alt="01-alerta-exfiltracao" src="https://github.com/user-attachments/assets/c146e360-f10c-4413-bf17-cc30df4c7a90" />

O volume transferido justificava a detecção, mas a origem associada a uma sala de reuniões e o destino do Zoom eram compatíveis com uso legítimo de videoconferência.

**Decisão no laboratório:** `False Positive` — confirmado pelo TryHackMe após encerrar o alerta.

No comentário, registrei essa interpretação e ressaltei que, em uma situação real, seria necessário confirmar a reunião e sua autorização antes de encerrar o caso apenas com esses campos.

## 3. Alerta de alta severidade — Double-Extension File Creation

O segundo alerta indicava a criação de um executável com dupla extensão. A extensão `.mp4.exe` pode fazer o arquivo parecer um vídeo quando o usuário não observa o nome completo.

| Campo | Valor |
|---|---|
| Severidade | High |
| Host | `LPT-HR-009` |
| Usuário | `S.Conway` |
| Processo relacionado | `chrome.exe` |
| Arquivo | `C:\Users\S.Conway\Downloads\cats2025.mp4.exe` |
| URL associada | `https://freecatvideoshd.monster/cats2025.mp4.exe` |

<img width="1187" height="406" alt="02-alerta-dupla-extensao" src="https://github.com/user-attachments/assets/9cfb5651-c0d1-4707-8a3a-195f1606ee94" />

O nome do arquivo, o formato executável e a origem indicada eram suficientes para tratar a criação como uma atividade suspeita no exercício.

**Decisão no laboratório:** `True Positive` — confirmado pelo TryHackMe após o encerramento.

A evidência disponibilizada mostra a criação do arquivo, **não sua execução nem uma infecção confirmada**. Essa limitação também foi registrada no comentário do analista.

## 4. Alerta de baixa severidade — Download from GitHub Repository

O terceiro alerta foi gerado por uma atividade de download relacionada ao GitHub.

| Campo | Valor |
|---|---|
| Severidade | Low |
| Usuário | `G.Chandler` |
| Host | `LPT-IT-063` |
| Rede | `VPN/DEVELOPERS` |
| URL acessada | `https://github.com/facebook/react` |

<img width="1256" height="333" alt="03-alerta-github" src="https://github.com/user-attachments/assets/0158228e-8301-4f71-94c1-04f319138338" />

O endereço corresponde ao projeto React, e o equipamento estava associado à rede de desenvolvedores. Esses detalhes tornavam a atividade compatível com um uso legítimo no contexto simulado.

**Decisão no laboratório:** `False Positive` — confirmada pelo TryHackMe.

No comentário, registrei que, em um ambiente real, seria importante verificar qual conteúdo foi baixado e se a atividade era autorizada.

## 5. Resultado e encerramento

Após a análise, defini o veredito de cada caso, escrevi um comentário com a justificativa e alterei o status para `Closed`.

| Alerta | Severidade | Veredito final |
|---|---|---|
| Potential Data Exfiltration | Critical | False Positive |
| Double-Extension File Creation | High | True Positive |
| Download from GitHub Repository | Low | False Positive |

<img width="1274" height="531" alt="04-veredito-confirmado" src="https://github.com/user-attachments/assets/781c783a-b2af-4c3d-bdef-024ac480c694" />

A plataforma validou os três vereditos e liberou as confirmações de conclusão. Os registros do exercício demonstram o uso do fluxo de triagem no painel, sem afirmar que foram consultados logs adicionais, ferramentas externas de inteligência ou sistemas corporativos reais.

## 6. Conclusão

A prática mostrou como uma fila de alertas pode conter situações muito diferentes: um volume alto de dados associado a uma videoconferência, um executável com nome enganoso e uma atividade de desenvolvimento no GitHub.

Também consegui registrar o atendimento completo de cada caso, desde a atribuição ao analista L1 até o encerramento com justificativa. O principal aprendizado foi não tratar **severidade**, **detecção** e **comprometimento confirmado** como sinônimos.

---

**[Voltar aos fundamentos de Segurança Defensiva](./fundamentos-seguranca-defensiva.md)**
