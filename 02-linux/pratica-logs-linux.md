# Prática — Investigação de Logs de Autenticação SSH no Linux

**Módulo 2 — Linux | Cyber Security 101 — TryHackMe**

## Objetivo

Aplicar os comandos básicos de Linux em uma análise de registros de autenticação SSH. A proposta foi identificar tentativas de acesso malsucedidas, comparar com autenticações aceitas e verificar se havia um padrão que merecesse investigação.

## Ambiente e ferramentas

- **Ambiente:** máquina Ubuntu disponibilizada pelo TryHackMe, na sala *Linux Logging for SOC*.
- **Arquivo analisado:** `/var/log/auth.log`.
- **Ferramentas:** terminal Linux e comandos `head`, `grep`, `sed`, `sort` e `uniq`.

## 1. Localização e leitura dos registros

Comecei verificando se o arquivo de autenticação estava disponível:

```bash
ls -l /var/log/auth.log
```

Depois, consultei as primeiras linhas para entender o formato dos registros:

```bash
sudo head -n 15 /var/log/auth.log
```

Encontrei mensagens de `sudo`, `su`, `pam_unix` e `CRON`, relacionadas a sessões, execução de comandos administrativos e tarefas agendadas. Nem todo registro de autenticação representa uma tentativa de invasão.

## 2. Investigação de falhas SSH

Pesquisei tentativas malsucedidas de autenticação utilizando:

```bash
sudo grep "sshd.*Failed password" /var/log/auth.log
```

Na captura, identifiquei tentativas direcionadas às contas `admin`, `support` e `root`, originadas do endereço `10.14.94.82`, em 13 de agosto de 2025. A concentração de tentativas em um intervalo curto é compatível com adivinhação de credenciais, mas não é prova de comprometimento.

**Evidência 1 — Tentativas de autenticação malsucedidas**

<img width="690" height="428" alt="01-falhas-autenticacao-ssh" src="https://github.com/user-attachments/assets/e123bdde-c03d-4013-9b4e-f20cbf50ffb9" />

## 3. Comparação com acessos aceitos

Em seguida, procurei autenticações SSH bem-sucedidas:

```bash
sudo grep "Accepted" /var/log/auth.log
```

Encontrei acessos à conta `ubuntu` por chave pública e senha, originados de outros endereços IP, em datas diferentes. Nos resultados examinados, não identifiquei autenticação aceita do mesmo IP responsável pelas falhas. Essa comparação, isoladamente, não permite concluir que o sistema esteja totalmente seguro.

**Evidência 2 — Autenticações SSH aceitas**

<img width="683" height="688" alt="02-acessos-ssh-aceitos" src="https://github.com/user-attachments/assets/45c2ec91-5ff5-4353-9753-600378913b3b" />

## 4. Contagem de falhas por endereço IP

Para quantificar as tentativas, filtrei os registros do `sshd`, extraí o IP de origem e contei as ocorrências:

```bash
sudo grep "sshd.*Failed password" /var/log/auth.log | sed -n 's/.* from \([0-9.]*\) port .*/\1/p' | sort | uniq -c
```

O resultado foi:

```text
6 10.14.94.82
```

Portanto, foram encontradas **seis tentativas SSH malsucedidas** provenientes de um mesmo IP no arquivo analisado. O endereço é privado e pertence ao contexto de laboratório; não foi classificado como um IP público malicioso.

**Evidência 3 — Contagem de falhas por IP**

<img width="683" height="168" alt="03-contagem-falhas-por-ip" src="https://github.com/user-attachments/assets/6a6cd765-1ec4-4d8c-8cb2-fa60b8d8d113" />

Durante a análise, percebi que uma busca genérica por `Failed password` também podia retornar registros do próprio `sudo` executado durante o exercício. Ajustei o filtro para selecionar especificamente eventos do `sshd`, evitando contagens incorretas.

## Conclusão

Essa prática me ajudou a usar comandos básicos do Linux para investigar um cenário de autenticação SSH. Consegui localizar o arquivo de logs, identificar falhas, comparar acessos aceitos e quantificar tentativas por endereço IP.

O principal aprendizado foi que a análise de segurança exige cuidado com filtros e contexto: um resultado incorreto pode surgir de um comando aparentemente simples, e várias falhas de login não comprovam, por si só, que uma conta foi comprometida. Foi uma primeira experiência prática de triagem de logs ligada às atividades de um analista SOC.
