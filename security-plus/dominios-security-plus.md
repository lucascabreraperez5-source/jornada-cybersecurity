# CompTIA Security+ (SY0-701) — Fundamentos dos 5 Domínios

> **Preparação para a certificação CompTIA Security+ | Em andamento**

## Sobre o projeto

Depois de estudar redes, Linux, Windows, Active Directory e realizar algumas práticas de segurança defensiva, comecei a me preparar para a certificação **CompTIA Security+ (SY0-701)**.

O objetivo é consolidar os conhecimentos adquiridos até agora e aprofundar assuntos relacionados a ameaças, arquitetura de segurança, operações de SOC e gerenciamento de riscos.

Minha preparação inclui conteúdos teóricos, laboratórios do TryHackMe e **simulados do Jason Dion, disponíveis na Udemy**. Utilizo os resultados para identificar os assuntos em que tenho mais dificuldade e direcionar minhas revisões.

Este documento reúne os principais conceitos dos cinco domínios da Security+, relacionando-os aos meus estudos e às atividades práticas documentadas no GitHub.

## Como estou estudando

- **Conteúdo teórico:** estudo e revisão dos principais conceitos da certificação.
- **Simulados Jason Dion:** resolução de questões no formato da prova para avaliar meus conhecimentos.
- **Revisão de erros:** análise das questões incorretas, buscando entender o conceito e não apenas memorizar respostas.
- **Anotações:** registro de diferenças importantes entre termos e assuntos recorrentes.
- **Laboratórios:** aplicação de conceitos em atividades práticas de segurança defensiva.

Uma das maiores dificuldades que identifiquei nos simulados foi a interpretação de questões que apresentam conceitos muito parecidos. Por isso, passei a organizar as correções por domínio e revisar principalmente os pontos em que ainda tenho dúvidas.

---

## 1. General Security Concepts — 12%

Este domínio aborda os princípios básicos utilizados para proteger sistemas e informações.

### Principais conceitos

| Conceito | O que significa |
|---|---|
| Tríade CIA | Confidencialidade, integridade e disponibilidade |
| AAA | Autenticação, autorização e registro de atividades |
| Security Controls | Controles preventivos, detectivos, corretivos e outros |
| Zero Trust | Não conceder confiança automática a usuários e dispositivos |
| Least Privilege | Fornecer apenas as permissões necessárias |
| Cryptography | Proteção de informações por mecanismos criptográficos |
| Change Management | Controle e documentação de alterações em sistemas |

Durante a revisão, procurei reforçar principalmente as diferenças entre **autenticação e autorização**, as categorias e funções dos controles de segurança e a aplicação da criptografia.

Também relacionei os conceitos de hashing e integridade com minha prática de verificação de arquivos utilizando SHA-256.

---

## 2. Threats, Vulnerabilities, and Mitigations — 22%

O segundo domínio aborda ameaças, agentes maliciosos, vulnerabilidades e medidas utilizadas para reduzir riscos.

### Principais conceitos

| Conceito | O que significa |
|---|---|
| Threat Actors | Pessoas ou grupos que representam ameaças |
| Attack Surface | Pontos que podem ser explorados em um ambiente |
| Social Engineering | Manipulação de pessoas para obter acessos ou informações |
| Malware | Softwares com comportamentos maliciosos |
| Web Attacks | Ataques como SQL Injection e XSS |
| Network Attacks | Ataques como DDoS, DNS Poisoning e On-Path |
| Vulnerability Mitigation | Medidas para reduzir a exposição a vulnerabilidades |

Uma parte importante dos estudos foi diferenciar técnicas de engenharia social, principalmente **phishing, spear phishing, whaling, smishing e vishing**.

Também revisei tipos de malware, ataques a aplicações web e formas comuns de exploração de vulnerabilidades.

### Relação com minhas práticas

Na investigação com Splunk e Sysmon, analisei a execução de um processo suspeito, sua conexão de rede e possíveis mecanismos de persistência.

Na prática de triagem SOC, avaliei alertas relacionados a exfiltração de dados, arquivos com dupla extensão e downloads de repositórios.

Essas atividades ajudaram a compreender como comportamentos suspeitos podem ser identificados e analisados.

---

## 3. Security Architecture — 18%

Este domínio apresenta conceitos utilizados para proteger infraestruturas, redes, aplicações e dados.

### Principais conceitos

| Conceito | O que significa |
|---|---|
| On-Premises e Cloud | Diferentes modelos de infraestrutura |
| Shared Responsibility | Divisão das responsabilidades entre cliente e provedor |
| Network Segmentation | Separação de redes para restringir comunicações |
| Firewall, IDS e IPS | Controle, detecção e prevenção de atividades suspeitas |
| WAF | Proteção específica para aplicações web |
| Data Protection | Proteção de dados armazenados, transmitidos e processados |
| High Availability | Recursos para reduzir interrupções e manter serviços disponíveis |

Também revisei conceitos relacionados a backup, snapshots, redundância e recuperação de desastres.

### RTO, RPO e MTTR

Esses três termos apareceram nas revisões dos simulados e exigiram atenção:

| Termo | Significado |
|---|---|
| **RTO** | Tempo máximo aceitável para restaurar um serviço |
| **RPO** | Perda máxima aceitável de dados, medida em tempo |
| **MTTR** | Tempo médio necessário para reparar ou restaurar um recurso |

A principal diferença é que RTO está relacionado ao tempo de recuperação, enquanto RPO está relacionado à quantidade de dados que pode ser perdida.

---

## 4. Security Operations — 28%

Este é o domínio com maior peso na certificação e um dos mais relacionados à minha preparação para trabalhar em um **SOC Júnior**.

Ele aborda monitoramento, gerenciamento de vulnerabilidades, controles de acesso e resposta a incidentes.

### Principais conceitos

| Conceito | O que significa |
|---|---|
| Hardening | Redução da superfície de ataque de sistemas |
| Vulnerability Management | Identificação, avaliação e tratamento de vulnerabilidades |
| SIEM | Centralização, pesquisa e correlação de eventos |
| EDR | Monitoramento e resposta em endpoints |
| IAM | Gerenciamento de identidades e acessos |
| Incident Response | Processo de identificação e tratamento de incidentes |
| SOAR | Automação e orquestração de atividades de segurança |

### Resposta a incidentes

Uma das partes importantes deste domínio é compreender as etapas de resposta a incidentes:

1. **Preparation:** preparar recursos e procedimentos.
2. **Detection and Analysis:** identificar e analisar atividades suspeitas.
3. **Containment:** limitar o impacto do incidente.
4. **Eradication:** eliminar componentes maliciosos ou causas identificadas.
5. **Recovery:** restaurar serviços e sistemas.
6. **Lessons Learned:** revisar o ocorrido e melhorar os controles.

### Relação com minhas práticas

Este domínio tem ligação direta com grande parte do meu portfólio:

- Análise de logs de autenticação no Linux.
- Investigação de eventos do Windows e Active Directory.
- Triagem de endpoints utilizando PowerShell.
- Análise de tráfego de rede no Wireshark.
- Investigação de eventos com Splunk e Sysmon.
- Classificação e documentação de alertas em um SOC simulado.

Essas atividades me ajudaram a visualizar como os conceitos de segurança operacional são aplicados na prática.

---

## 5. Security Program Management and Oversight — 20%

O quinto domínio aborda políticas, gerenciamento de riscos, conformidade e processos organizacionais de segurança.

Foi um dos domínios em que precisei revisar várias diferenças entre conceitos semelhantes durante os simulados.

### Principais conceitos

| Conceito | O que significa |
|---|---|
| Governance | Diretrizes e responsabilidades de segurança |
| Risk Management | Identificação, avaliação e tratamento de riscos |
| Risk Assessment | Processo de identificação, análise e avaliação de riscos |
| Risk Analysis | Análise de probabilidade, impacto e características dos riscos |
| Compliance | Atendimento a requisitos legais, regulatórios e contratuais |
| Security Audits | Verificação de controles e requisitos |
| Third-Party Risk | Riscos relacionados a fornecedores e parceiros |

### Conceitos que mais revisei

| Termos | Diferença principal |
|---|---|
| Risk Assessment x Risk Analysis | Avaliação de riscos é mais ampla; análise é uma de suas etapas |
| Qualitative x Quantitative | Classificações descritivas versus estimativas numéricas |
| KRI x KPI | Indicador de risco versus indicador de desempenho |
| BCP x IRP | Continuidade de negócios versus resposta a incidentes |
| RTO x RPO | Tempo de recuperação versus perda aceitável de dados |
| Ad Hoc x Periodic Assessment | Avaliação conforme necessidade versus avaliação programada |

Também estudei conceitos de aceitação, mitigação, transferência e prevenção de riscos, além de regulamentações como LGPD e GDPR.

Esses assuntos me ajudaram a entender que segurança da informação envolve não apenas ferramentas, mas também decisões de negócio, prioridades e responsabilidades.

---

## Revisão dos simulados do Jason Dion

Os simulados do Jason Dion têm sido uma das principais ferramentas da minha preparação.

No primeiro simulado, identifiquei dificuldades especialmente nos fundamentos gerais de segurança e em algumas questões situacionais. Isso me ajudou a perceber quais assuntos precisavam de mais atenção antes de avançar.

Passei então a revisar as questões por domínio, separando os conceitos que errei ou em que tive dúvidas.

Minha intenção não é decorar as alternativas, mas compreender por que determinada resposta é correta e por que as outras não se aplicam ao cenário apresentado.

Entre os assuntos que precisei reforçar estão:

- Tipos e funções dos controles de segurança.
- Engenharia social e diferenças entre ataques.
- Arquitetura e disponibilidade de sistemas.
- Resposta a incidentes e continuidade de negócios.
- Avaliação de riscos e indicadores.
- Interpretação de cenários e termos semelhantes.

Continuo utilizando os simulados para acompanhar minha evolução e direcionar os estudos para os assuntos que ainda preciso melhorar.

---

## Principais aprendizados

A preparação para a Security+ está me ajudando a organizar os conhecimentos adquiridos durante minha jornada em cibersegurança.

Os principais aprendizados até agora foram:

- Compreender como os cinco domínios se relacionam.
- Diferenciar ameaças, vulnerabilidades e riscos.
- Reconhecer controles e tecnologias de segurança.
- Entender princípios de arquitetura e continuidade.
- Relacionar monitoramento e resposta a incidentes às atividades de um SOC.
- Compreender a importância de políticas, auditorias e gerenciamento de riscos.
- Utilizar os erros dos simulados para melhorar minha preparação.

Também percebi que resolver questões exige mais do que memorizar definições. É necessário interpretar o cenário e identificar o conceito mais adequado.

---

## Aplicação em Cibersegurança

Meu objetivo é ingressar na área de **Segurança Defensiva (Blue Team)**, preferencialmente em uma posição inicial de SOC.

A preparação para a Security+ complementa minhas atividades práticas ao reunir conhecimentos de redes, sistemas operacionais, criptografia, ameaças, monitoramento e gestão de segurança.

Continuo estudando os cinco domínios e realizando simulados para fortalecer minha base técnica antes do exame oficial.

---

## Conclusão

A CompTIA Security+ representa uma etapa importante da minha preparação para a primeira oportunidade profissional em cibersegurança.

Por meio dos conteúdos estudados, laboratórios e simulados do Jason Dion, estou conseguindo identificar dificuldades, revisar conceitos e relacionar a teoria às práticas que já documentei no GitHub.

**A certificação ainda está em preparação**, e este documento registra os principais fundamentos que venho desenvolvendo ao longo dos estudos.

---

## Referências

- [CompTIA Security+ — Página oficial](https://www.comptia.org/certifications/security)
- [Jason Dion — Dion Training](https://www.diontraining.com/)
- [TryHackMe](https://tryhackme.com/)
