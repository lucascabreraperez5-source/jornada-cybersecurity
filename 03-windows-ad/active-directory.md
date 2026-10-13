# Windows e Active Directory — Fundamentos para Segurança Corporativa

## Sobre o projeto

Este estudo faz parte da minha jornada em Cibersegurança pela trilha **Cyber Security 101**, na plataforma TryHackMe.

O objetivo deste módulo foi entender melhor o ambiente Windows, bastante presente em empresas, e conhecer o Active Directory (AD), uma tecnologia fundamental para o gerenciamento de usuários e acessos em redes corporativas.

## O que estudei

Ao longo das salas *Windows Fundamentals Part 1, 2 e 3* e *Active Directory Basics*, explorei conceitos importantes para compreender como funciona a infraestrutura Windows e como ela pode ser monitorada do ponto de vista da segurança.

Durante os estudos, trabalhei com:

- **Fundamentos do Windows:** recursos do sistema operacional e ferramentas administrativas.
- **Event Viewer (Visualizador de Eventos):** ferramenta utilizada para consultar registros de atividades e eventos do sistema.
- **Active Directory (AD):** gerenciamento centralizado de usuários, grupos, computadores e permissões em ambientes corporativos.
- **Domínios e Group Policy:** conceitos relacionados à organização de uma rede e à aplicação de políticas de segurança.

## Conceitos importantes

### Active Directory

O Active Directory permite que uma organização gerencie centralmente identidades, computadores e permissões dentro de um ambiente Windows corporativo.

Por concentrar informações e controles importantes de acesso, ele é um alvo valioso para invasores. O comprometimento de contas privilegiadas ou de componentes críticos do AD pode abrir caminho para o acesso não autorizado a diversos recursos da rede.

### Event Viewer e análise de eventos

O Visualizador de Eventos reúne registros que ajudam a entender o que aconteceu em um sistema Windows.

Dois eventos importantes que conheci foram:

- **Event ID 4624:** registro de uma tentativa de logon bem-sucedida.
- **Event ID 4625:** registro de uma tentativa de logon com falha.

Esses registros podem ajudar na investigação de atividades suspeitas, principalmente quando analisados em conjunto com outros eventos e informações do ambiente.

## Principais aprendizados

Esse módulo me ajudou a entender melhor como funciona a estrutura de acesso de uma rede corporativa e onde encontrar informações que podem auxiliar na investigação de incidentes.

Também percebi que analisar um evento isoladamente nem sempre é suficiente. É importante considerar o contexto, a frequência das ocorrências e a relação entre diferentes registros para avaliar se uma atividade merece investigação.

## Aplicação em Cibersegurança

O conhecimento de Windows, Active Directory e logs é uma base importante para quem pretende trabalhar com **Segurança Defensiva (Blue Team), especialmente em um SOC Nível 1**.

Esses fundamentos ajudam a compreender alertas relacionados a falhas de autenticação, acessos suspeitos e possíveis tentativas de comprometimento de contas corporativas.

## Próximos passos

Continuar praticando a análise de eventos do Windows, aprofundar os conhecimentos sobre Active Directory e avançar para laboratórios que envolvam detecção e investigação de atividades suspeitas.
