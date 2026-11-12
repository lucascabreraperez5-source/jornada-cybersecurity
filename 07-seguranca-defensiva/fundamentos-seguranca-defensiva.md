# Fundamentos de Segurança Defensiva — Triagem de Alertas em SOC

## Sobre o projeto

Este estudo faz parte do Módulo 10 da trilha Cyber Security 101, na plataforma TryHackMe.

Depois de estudar Windows, Linux, redes e análise de logs com Splunk, passei a explorar uma etapa importante da rotina de um SOC: a triagem de alertas. O objetivo deste projeto foi compreender como um analista organiza a fila de alertas, avalia o contexto e registra uma decisão a partir das informações disponíveis.

Utilizei o laboratório **SOC L1 Alert Triage**, do TryHackMe, que apresenta um painel SOC simulado com alertas de diferentes severidades.

## O que estudei

- Responsabilidades de um analista SOC Nível 1.
- Diferença entre evento, alerta e incidente.
- Campos mais importantes de um alerta.
- Priorização considerando severidade, contexto e impacto.
- Diferença entre verdadeiro positivo e falso positivo.
- Processo de atribuição, análise, documentação e encerramento de alertas.
- Limites de uma investigação quando os registros disponíveis são insuficientes.

---

## 1. O que é um SOC?

SOC (*Security Operations Center*) é a estrutura responsável por acompanhar sinais de ameaças e apoiar a detecção, investigação e resposta a incidentes. Uma equipe SOC pode usar ferramentas como SIEM e EDR para centralizar informações e acompanhar comportamentos suspeitos.

O analista **SOC L1** normalmente realiza a triagem inicial: examina alertas, verifica informações básicas, registra o que encontrou e encaminha casos que exigem análise aprofundada. O fluxo real varia conforme a organização e seus procedimentos.

## 2. Evento, alerta e incidente

| Conceito | Significado | Exemplo |
|---|---|---|
| Evento | Registro de uma atividade observada | Um arquivo foi criado |
| Alerta | Sinal gerado por uma regra ou ferramenta | Criação de arquivo com dupla extensão |
| Incidente | Situação que compromete ou ameaça a segurança e exige tratamento | Execução confirmada de código malicioso |

Um alerta informa que algum critério de detecção foi atendido. **Isso não significa, automaticamente, que houve ataque.** A triagem serve justamente para verificar o contexto.

## 3. Informações importantes em um alerta

No painel do laboratório, observei campos utilizados na rotina de atendimento:

| Campo | Para que serve |
|---|---|
| Name | Identifica o tipo de alerta |
| Severity | Indica a severidade atribuída |
| Status | Mostra a etapa do atendimento |
| Verdict | Registra a conclusão do analista |
| Assignee | Identifica o responsável pelo caso |
| Analyst Comment | Guarda a justificativa da análise |

O conteúdo do alerta também pode incluir usuário, computador, IP de origem, destino, arquivo envolvido e quantidade de dados transferidos.

## 4. Como funciona a priorização

Nem sempre o alerta mais recente é o mais urgente. Na triagem, é importante considerar a severidade, o possível impacto, a criticidade do sistema afetado e a confiança nas evidências.

Por exemplo, um alerta **Critical** sobre possível saída indevida de dados merece atenção antes de um alerta **Low** sobre acesso a um repositório, caso não existam outras circunstâncias que alterem essa prioridade.

A severidade indicada pela ferramenta é um ponto de partida. A avaliação pode mudar quando o contexto fica mais claro.

## 5. Verdadeiro positivo e falso positivo

- **True Positive (verdadeiro positivo):** a atividade suspeita descrita pela regra realmente ocorreu e o alerta é considerado válido. Isso não significa necessariamente que todas as consequências de um ataque estejam comprovadas.
- **False Positive (falso positivo):** a regra sinalizou uma atividade que, após análise, foi considerada legítima ou não representava a ameaça sugerida.

Um exemplo do laboratório foi um alerta de acesso à VPN a partir de um país incomum. A própria atividade existiu, mas havia uma justificativa confirmada pela usuária, e o caso já estava registrado como falso positivo.

## 6. Processo básico de triagem

O fluxo apresentado no laboratório foi:

1. **Priorizar:** identificar qual alerta deve ser tratado primeiro.
2. **Assumir:** atribuir o alerta ao analista responsável.
3. **Iniciar:** alterar seu status para `In Progress`.
4. **Ler os detalhes:** identificar o que disparou a regra e os campos relevantes.
5. **Investigar:** verificar contexto, eventos relacionados e possíveis explicações.
6. **Decidir:** registrar o veredito com base nas evidências.
7. **Documentar:** explicar a conclusão no comentário do analista.
8. **Encerrar ou escalar:** seguir o procedimento de resposta adequado.

Em um ambiente real, podem existir outras etapas, como consulta a ferramentas adicionais, confirmação com o usuário, preservação de evidências e escalonamento para L2.

## 7. Três tipos de alertas observados

### Possível exfiltração de dados

O sistema detectou o envio de mais de 5 GB de dados. O volume é relevante, mas seu significado depende do destino e da finalidade da comunicação. No exercício, o equipamento estava associado a uma sala de reuniões e o destino era `*.zoom.us`, cenário compatível com videoconferência.

### Arquivo com dupla extensão

Um nome como `cats2025.mp4.exe` pode induzir o usuário a pensar que se trata de um vídeo, embora a extensão final indique um executável. Esse padrão merece investigação, principalmente quando o arquivo vem de um endereço suspeito. A criação do arquivo, por si só, não comprova sua execução.

### Download de repositório no GitHub

O GitHub hospeda projetos legítimos e também pode conter conteúdo perigoso. Por isso, a reputação da plataforma não basta para concluir que toda atividade é segura. No laboratório, o endereço era o repositório público do React, acessado de um equipamento associado à rede de desenvolvedores.

## 8. Principais aprendizados

O exercício ajudou a compreender que **a classificação de um alerta depende do contexto e das evidências**, não somente do nome da detecção ou da severidade exibida.

Também observei a importância de registrar quem assumiu o caso, manter o status atualizado e escrever comentários que permitam a outra pessoa entender a conclusão. No laboratório, trabalhei com os dados apresentados pelo painel e com os vereditos validados pela própria plataforma; não houve acesso a uma infraestrutura corporativa real.

## 9. Aplicação em Cibersegurança

A triagem é uma habilidade diretamente relacionada à preparação para SOC Júnior. Ela reúne conhecimentos de redes, sistemas operacionais, identificação de indicadores e documentação técnica.

Pretendo continuar desenvolvendo essa habilidade em laboratórios mais completos, com consultas de logs e análise de evidências adicionais, para ganhar autonomia na investigação de alertas.

---

## Atividade prática

Registrei a triagem de três alertas no painel SOC simulado do TryHackMe, incluindo a prioridade, os principais indicadores, os vereditos e os comentários de encerramento.

**[Ver prática — Triagem de Alertas em SOC](./pratica-triagem-soc.md)**
