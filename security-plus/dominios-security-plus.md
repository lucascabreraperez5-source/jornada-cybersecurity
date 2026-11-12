# CompTIA Security+ (SY0-701) — Fundamentos dos 5 Domínios

> **Preparação para a certificação CompTIA Security+ | Em andamento**

## Sobre o projeto

Depois de estudar redes, Linux, Windows, Active Directory e realizar práticas de segurança defensiva, comecei minha preparação para a certificação **CompTIA Security+ (SY0-701)**.

O objetivo é aprofundar conhecimentos em segurança da informação, principalmente sobre ameaças, arquitetura de segurança, operações de SOC e gerenciamento de riscos.

Este documento reúne os principais conceitos dos cinco domínios da certificação, que estou estudando como parte da minha preparação para ingressar na área de segurança defensiva (Blue Team).

## Metodologia de estudo

Minha preparação combina conteúdo teórico, laboratórios do TryHackMe e **simulados do Jason Dion, disponíveis na Udemy**.

Após cada simulado, reviso as questões que errei ou respondi com dúvida, organizando as anotações por domínio. Procuro entender o motivo da resposta correta e as diferenças entre conceitos semelhantes, em vez de apenas memorizar alternativas.

No primeiro simulado, identifiquei dificuldades especialmente nos fundamentos gerais de segurança e na interpretação de algumas questões. A partir disso, passei a direcionar as revisões para os assuntos que mais precisavam de atenção.

Entre os conceitos que já revisei estão tipos de controles de segurança, engenharia social, continuidade de negócios, avaliação de riscos e indicadores de segurança.

---

## 1. General Security Concepts — 12%

O primeiro domínio apresenta os princípios básicos utilizados para proteger informações, sistemas e serviços.

| Conceito | Descrição |
|---|---|
| **Tríade CIA** | Confidencialidade, integridade e disponibilidade |
| **AAA** | Autenticação, autorização e registro de atividades |
| **Security Controls** | Controles preventivos, detectivos, corretivos e outros |
| **Zero Trust** | Acesso baseado em verificação, sem confiança automática |
| **Least Privilege** | Conceder somente as permissões necessárias |
| **Cryptography** | Proteção de informações por mecanismos criptográficos |
| **Change Management** | Planejamento, aprovação e controle de mudanças |

### Pontos importantes

A tríade CIA representa três objetivos fundamentais: impedir acessos indevidos, preservar a integridade das informações e manter sistemas disponíveis.

Também é importante diferenciar **autenticação**, que confirma uma identidade, de **autorização**, que determina quais recursos ela pode acessar.

Os controles de segurança podem ter diferentes funções. Um firewall pode atuar como controle preventivo, enquanto uma solução de monitoramento pode exercer uma função detectiva.

No conteúdo de criptografia, revisei as diferenças entre criptografia simétrica, assimétrica e hashing, incluindo o uso do SHA-256 para verificação de integridade.

---

## 2. Threats, Vulnerabilities, and Mitigations — 22%

Este domínio aborda agentes de ameaça, técnicas de ataque, vulnerabilidades e formas de reduzir riscos.

| Conceito | Descrição |
|---|---|
| **Threat Actors** | Pessoas ou grupos capazes de representar ameaças |
| **Attack Surface** | Pontos de um sistema que podem ser explorados |
| **Social Engineering** | Manipulação de pessoas para obter acessos ou informações |
| **Malware** | Softwares desenvolvidos para realizar atividades maliciosas |
| **Web Attacks** | Ataques como SQL Injection e XSS |
| **Network Attacks** | DDoS, DNS Poisoning e ataques On-Path |
| **Mitigation** | Medidas para reduzir a probabilidade ou o impacto de ameaças |

### Pontos importantes

Entre os tipos de engenharia social, é necessário diferenciar:

- **Phishing:** mensagens fraudulentas utilizadas para enganar usuários.
- **Spear Phishing:** ataque direcionado a uma pessoa ou grupo específico.
- **Whaling:** ataque direcionado a executivos ou pessoas de alto valor.
- **Vishing:** engenharia social realizada por voz.
- **Smishing:** phishing realizado por SMS ou mensagens semelhantes.

Também estudei diferentes tipos de malware, como ransomware, trojans, worms e spyware, além de vulnerabilidades de aplicações e ataques de rede.

As principais medidas de mitigação incluem atualizações, hardening, segmentação de redes, MFA e controle de privilégios.

---

## 3. Security Architecture — 18%

O terceiro domínio trata da proteção de infraestruturas, redes, aplicações e dados.

| Conceito | Descrição |
|---|---|
| **On-Premises e Cloud** | Diferentes modelos de infraestrutura |
| **Shared Responsibility** | Responsabilidades de segurança entre cliente e provedor |
| **Network Segmentation** | Divisão de redes para limitar acessos e comunicações |
| **Firewall, IDS e IPS** | Controle, detecção e prevenção de atividades suspeitas |
| **WAF** | Proteção de aplicações web |
| **Data Protection** | Proteção de dados armazenados, transmitidos e processados |
| **High Availability** | Redundância e recursos para manter serviços disponíveis |

### Continuidade e recuperação

Uma parte importante deste domínio envolve garantir a disponibilidade de sistemas e a recuperação após interrupções.

| Termo | Significado |
|---|---|
| **RTO** | Tempo máximo aceitável para restaurar um serviço |
| **RPO** | Perda máxima aceitável de dados, medida em tempo |
| **MTTR** | Tempo médio necessário para reparar ou restaurar um recurso |

A diferença entre RTO e RPO apareceu nas revisões dos simulados: o primeiro está relacionado ao tempo de recuperação, enquanto o segundo está relacionado à quantidade de dados que a organização pode perder.

Também fazem parte deste domínio conceitos como backups, snapshots, redundância, virtualização e proteção de ambientes em nuvem.

---

## 4. Security Operations — 28%

Este é o domínio com maior peso na certificação e possui relação direta com atividades de monitoramento e resposta a incidentes em um SOC.

| Conceito | Descrição |
|---|---|
| **Hardening** | Redução da superfície de ataque de sistemas |
| **Vulnerability Management** | Identificação, priorização e tratamento de vulnerabilidades |
| **SIEM** | Centralização, pesquisa e correlação de eventos |
| **EDR** | Monitoramento e resposta em endpoints |
| **IAM** | Gerenciamento de identidades e acessos |
| **Incident Response** | Identificação, contenção e tratamento de incidentes |
| **SOAR** | Automação e orquestração de atividades de segurança |

### Resposta a incidentes

O processo de resposta a incidentes pode envolver:

1. **Preparation:** preparação de ferramentas, equipes e procedimentos.
2. **Detection and Analysis:** identificação e análise de possíveis incidentes.
3. **Containment:** limitação dos impactos.
4. **Eradication:** remoção das causas ou componentes maliciosos.
5. **Recovery:** recuperação de sistemas e serviços.
6. **Lessons Learned:** revisão do incidente e melhoria dos controles.

Este domínio se relaciona diretamente com algumas práticas que já documentei no GitHub, como análise de logs Linux, eventos do Windows, investigação com Splunk e Sysmon, análise de tráfego no Wireshark e triagem de alertas SOC.

---

## 5. Security Program Management and Oversight — 20%

O quinto domínio aborda governança, gerenciamento de riscos, conformidade e processos organizacionais de segurança.

| Conceito | Descrição |
|---|---|
| **Governance** | Políticas, diretrizes e responsabilidades de segurança |
| **Risk Management** | Identificação, avaliação e tratamento de riscos |
| **Risk Assessment** | Processo amplo de identificação, análise e avaliação de riscos |
| **Risk Analysis** | Estimativa de características como probabilidade e impacto |
| **Compliance** | Atendimento a requisitos legais, regulatórios e contratuais |
| **Security Audits** | Verificação de controles e requisitos |
| **Third-Party Risk** | Riscos relacionados a fornecedores e parceiros |

### Conceitos revisados nos simulados

Este domínio apresentou algumas diferenças importantes entre termos semelhantes:

| Termos | Diferença principal |
|---|---|
| **Risk Assessment x Risk Analysis** | A avaliação é mais ampla; a análise faz parte do processo |
| **Qualitative x Quantitative** | Classificações descritivas versus estimativas numéricas |
| **KRI x KPI** | Indicador de risco versus indicador de desempenho |
| **BCP x IRP** | Continuidade de negócios versus resposta a incidentes |
| **Ad Hoc x Periodic Assessment** | Avaliação conforme necessidade versus avaliação programada |

Também revisei estratégias de tratamento de riscos, como aceitação, mitigação, transferência e prevenção, além de conceitos relacionados a auditorias e regulamentações como LGPD e GDPR.

---

## Relação com as atividades práticas

Os estudos da Security+ complementam os projetos que já desenvolvi durante minha jornada em cibersegurança.

Nos laboratórios, tive contato com análise de logs, investigação de processos, protocolos de rede, controles de acesso, monitoramento com SIEM e triagem de alertas.

A certificação reúne esses assuntos em uma estrutura mais ampla, incluindo também arquitetura, gestão de riscos e governança de segurança.

---

## Conclusão

A preparação para a **CompTIA Security+ SY0-701** faz parte do meu desenvolvimento para buscar uma primeira oportunidade em segurança defensiva.

Continuo estudando os cinco domínios e utilizando os simulados do Jason Dion para identificar dificuldades e direcionar as revisões.

**A certificação ainda está em preparação**, e este documento registra os principais fundamentos que venho consolidando ao longo dos estudos.

---

## Referências

- [CompTIA Security+ — Página oficial](https://www.comptia.org/certifications/security)
- [Jason Dion — Dion Training](https://www.diontraining.com/)
- [TryHackMe](https://tryhackme.com/)
