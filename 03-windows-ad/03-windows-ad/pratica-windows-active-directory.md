# Prática — Investigação de Eventos do Windows e Fundamentos de Active Directory

**Módulo 3 — Windows e Active Directory | Cyber Security 101 — TryHackMe**

## Objetivo

Aplicar os conceitos estudados sobre Windows e Active Directory em duas atividades práticas: analisar registros de autenticação no Windows e explorar usuários, grupos e políticas de segurança em um domínio de laboratório do TryHackMe.

As atividades foram realizadas em **ambientes diferentes**. No meu computador pessoal, utilizei o Event Viewer. No TryHackMe, explorei o domínio de treinamento `thm.local`, sem alterar suas configurações.

## 1. Windows — Análise de eventos de autenticação

Comecei utilizando o **Event Viewer** (`eventvwr.msc`), acessando **Windows Logs → Security** e filtrando os eventos:

- **4624:** logon realizado com sucesso.
- **4625:** tentativa de logon malsucedida.

Ao analisar um evento 4624, identifiquei o **Logon Type 5**, relacionado ao logon de um serviço do Windows, e o **Logon ID `0x3E7`**, associado ao contexto da conta SYSTEM.

**Evidência 1 — Evento 4624 e Logon Type 5**

![Evento 4624 com Logon Type 5](https://github.com/user-attachments/assets/8cd45302-be21-4de8-b7b0-c2021b0eef2d)

Em seguida, procurei um evento **4672**, que registra a atribuição de privilégios especiais durante um logon.

Encontrei um registro com o mesmo horário (22:09:19), conta SYSTEM e Logon ID `0x3E7` do evento 4624 analisado.

**Evidência 2 — Evento 4672 e comparação do Logon ID**

![Evento 4672 com Logon ID correspondente](https://github.com/user-attachments/assets/ec8befe9-c888-44c0-a839-7ac87f47bc8b)

A combinação dessas informações é compatível com a atividade de um serviço do sistema. Entretanto, como o identificador `0x3E7` é frequentemente utilizado pela conta SYSTEM, essa comparação não comprova, isoladamente, uma única sequência de autenticação.

Também consultei especificamente os eventos **4625**, mas não encontrei registros correspondentes no histórico disponível.

**O que aprendi:** encontrar um evento é apenas o começo da análise. Para interpretá-lo corretamente, é necessário considerar o tipo de logon, a conta envolvida, o horário e o contexto da atividade, evitando classificar comportamentos comuns do sistema como ameaças sem evidências.

## 2. Active Directory — Usuários, grupos e permissões

No laboratório **Active Directory Basics**, do TryHackMe, utilizei a ferramenta **Active Directory Users and Computers** (`dsa.msc`) para explorar o domínio `thm.local`.

Na unidade organizacional **THM**, encontrei departamentos como IT, Management, Marketing, Research and Development e Sales.

Ao acessar a OU **IT**, identifiquei três contas de usuários: Claire, Mary e Philip.

**Evidência 3 — Estrutura organizacional e usuários da OU IT**

![Estrutura organizacional do Active Directory](https://github.com/user-attachments/assets/11acffb9-c0a4-4413-8a7c-9e5c6fd38036)

Depois, abri as propriedades da conta **Claire** e consultei a aba **Member Of**, identificando sua associação direta ao grupo **Domain Users**.

**Evidência 4 — Grupos da usuária Claire**

![Associação da usuária Claire ao grupo Domain Users](https://github.com/user-attachments/assets/d7ab5f75-3bd2-4dbe-a3f2-b8d01f6f4166)

Para comparar uma conta comum com uma conta privilegiada, consultei o grupo **Domain Admins** e verifiquei seus membros.

No laboratório, encontrei a conta **Administrator** como membro direto desse grupo.

**Evidência 5 — Membros do grupo Domain Admins**

![Membros do grupo privilegiado Domain Admins](https://github.com/user-attachments/assets/3cf01649-d083-4e34-845e-e313b3cf580a)

**O que aprendi:** uma Unidade Organizacional (OU) permite organizar objetos do domínio e administrar políticas, enquanto os grupos de segurança podem ser utilizados para conceder permissões.

Também entendi que pertencer à OU IT não significa automaticamente possuir privilégios administrativos e que a associação a grupos como Domain Admins merece atenção especial em ambientes corporativos.

## 3. Active Directory — Análise de políticas de segurança (GPO)

Na última etapa, abri o **Group Policy Management** (`gpmc.msc`) e analisei as configurações da **Default Domain Policy** do domínio `thm.local`.

### Política de senhas

Na seção **Password Policy**, encontrei as seguintes configurações:

| Configuração | Valor identificado |
|---|---|
| Comprimento mínimo da senha | 7 caracteres |
| Exigência de complexidade | Habilitada |
| Histórico de senhas | 24 senhas anteriores |
| Validade máxima da senha | 42 dias |
| Validade mínima da senha | 1 dia |
| Armazenamento com criptografia reversível | Desabilitado |

**Evidência 6 — Configurações da política de senhas**

![Configurações da Password Policy](https://github.com/user-attachments/assets/b81df165-f202-4579-9fe4-5a8c69b9f451)

### Política de bloqueio de contas

Também examinei a seção **Account Lockout Policy**, onde encontrei:

**Account lockout threshold: 0 invalid logon attempts**

Essa configuração indica que o bloqueio automático por quantidade de tentativas incorretas está desabilitado nessa política.

**Evidência 7 — Política de bloqueio de contas**

![Configuração da Account Lockout Policy](https://github.com/user-attachments/assets/8e523cdd-38ff-423c-89e2-ab26e37cd21f)

**O que observei:** o comprimento mínimo de 7 caracteres e a ausência de bloqueio automático por tentativas incorretas são configurações que merecem atenção em uma avaliação de segurança.

Entretanto, esses resultados não permitem concluir que todo o domínio esteja vulnerável, pois outras políticas e mecanismos de proteção também precisam ser considerados.

## Conclusão

Essa prática me ajudou a conectar dois assuntos importantes para a Segurança Defensiva: **investigação de eventos no Windows e gerenciamento de identidades e políticas no Active Directory**.

No Windows, aprendi a interpretar registros de autenticação e comparar eventos relacionados, sem tirar conclusões precipitadas.

Já no laboratório do TryHackMe, consegui explorar a estrutura de um domínio, verificar associações a grupos comuns e privilegiados e analisar configurações de segurança por meio de GPOs.

O principal aprendizado foi perceber que uma investigação não depende apenas de encontrar um evento ou uma configuração, mas de **entender seu contexto e avaliar o que aquela informação realmente representa para a segurança do ambiente**.
