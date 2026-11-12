# Prática — Verificação de Integridade de Arquivos com SHA-256

> **Criptografia | Windows PowerShell**

## Sobre a prática

Nesta atividade, utilizei o Windows PowerShell para verificar a integridade de um arquivo por meio do algoritmo SHA-256.

O objetivo foi observar como a modificação de um arquivo altera seu hash e como essa diferença pode ser identificada automaticamente.

Realizei o procedimento em meu próprio ambiente Windows, utilizando um arquivo de texto criado especificamente para o laboratório.

## Ambiente utilizado

- **Sistema operacional:** Windows
- **Ferramenta:** Windows PowerShell 5.1
- **Algoritmo:** SHA-256
- **Arquivo analisado:** `evidencia.txt`
- **Diretório:** `$HOME\lab-integridade`

---

## 1. Preparação do ambiente

Primeiro, criei uma pasta para organizar os arquivos utilizados na atividade.

```powershell
New-Item -ItemType Directory -Path "$HOME\lab-integridade" -Force
```

O comando `New-Item` cria um novo item no sistema. Utilizei `-ItemType Directory` para indicar que queria criar uma pasta e `-Force` para permitir sua reutilização caso já existisse.

Em seguida, criei um arquivo de texto:

```powershell
"Arquivo original para análise de integridade" | Set-Content "$HOME\lab-integridade\evidencia.txt"
```

O comando `Set-Content` gravou o texto informado no arquivo `evidencia.txt`.

Dessa forma, preparei um arquivo simples cujo conteúdo poderia ser modificado sem afetar outros dados do computador.

---

## 2. Cálculo do hash original

Com o arquivo criado, utilizei o seguinte comando:

```powershell
Get-FileHash "$HOME\lab-integridade\evidencia.txt" -Algorithm SHA256
```

O PowerShell calculou o hash SHA-256 do arquivo.

**Hash original obtido:**

```text
D1C7DF4FF1386D9330612C0F8D3D91CA201494170C3A585702005837C4BBB906
```

Esse resultado passou a ser minha referência para verificar se o conteúdo do arquivo seria alterado.

### Evidência 1 — Hash original

<img width="1104" height="593" alt="01-hash-original" src="https://github.com/user-attachments/assets/7b6170e2-243b-4497-9fa1-b2c332e3e3b2" />

---

## 3. Modificação do arquivo

Depois de registrar o hash original, adicionei uma nova linha ao mesmo arquivo:

```powershell
Add-Content "$HOME\lab-integridade\evidencia.txt" "Alteracao realizada durante a investigacao"
```

O comando `Add-Content` acrescentou o texto ao arquivo sem apagar seu conteúdo anterior.

Em seguida, executei novamente:

```powershell
Get-FileHash "$HOME\lab-integridade\evidencia.txt" -Algorithm SHA256
```

**Novo hash obtido:**

```text
44214BA34A31E2286A78E640D6F455CC81DABEEB35B9178FCD7B4A85CFF67B7E
```

Observei que o resultado era completamente diferente do valor calculado inicialmente.

### Comparação dos resultados

| Etapa | Resultado |
|---|---|
| Antes da modificação | `D1C7DF4F...C4BBB906` |
| Depois da modificação | `44214BA3...CFF67B7E` |
| Comparação | Hashes diferentes |

Essa diferença confirmou que o conteúdo do arquivo havia sido modificado.

### Evidência 2 — Comparação dos hashes

<img width="1132" height="372" alt="02-comparacao-hashes" src="https://github.com/user-attachments/assets/46f12f7f-6d35-498f-ab61-312260861faa" />

---

## 4. Verificação automática de integridade

Para complementar a atividade, realizei uma comparação automática utilizando variáveis e uma condição no PowerShell.

```powershell
$original = "D1C7DF4FF1386D9330612C0F8D3D91CA201494170C3A585702005837C4BBB906"

$atual = (Get-FileHash "$HOME\lab-integridade\evidencia.txt" -Algorithm SHA256).Hash

if ($original -eq $atual) {
    "Integridade preservada: hashes iguais"
} else {
    "Alteracao detectada: hashes diferentes"
}
```

### Entendendo o comando

- `$original`: armazena o hash calculado antes da modificação.
- `$atual`: recebe o hash atual do arquivo.
- `-eq`: verifica se os dois valores são iguais.
- `if`: executa uma ação quando a comparação é verdadeira.
- `else`: executa uma ação quando os valores são diferentes.

**Resultado apresentado pelo PowerShell:**

```text
Alteracao detectada: hashes diferentes
```

A comparação identificou corretamente que o arquivo não correspondia mais ao conteúdo original.

### Evidência 3 — Detecção automática da alteração

<img width="1129" height="128" alt="03-verificacao-integridade" src="https://github.com/user-attachments/assets/8c68d9c4-7ef8-4f40-afc1-9d6ab063a324" />

---

## 5. Resultado da atividade

Ao final do laboratório, consegui:

- Criar um arquivo de teste utilizando PowerShell.
- Calcular seu hash SHA-256.
- Registrar o valor original como referência.
- Modificar o conteúdo do arquivo.
- Calcular um novo hash.
- Comparar os valores automaticamente.
- Identificar a alteração realizada.

O resultado demonstrou que uma função hash pode ser utilizada para detectar modificações em arquivos.

É importante destacar que o laboratório não envolveu um ataque real ou uma alteração maliciosa. A modificação foi realizada intencionalmente para observar o comportamento do SHA-256.

---

## 6. Aplicação em Segurança Defensiva

Em uma investigação de segurança, a comparação de hashes pode ajudar um analista a verificar se determinado arquivo corresponde a uma versão de referência confiável.

Essa técnica pode ser utilizada em situações como:

- Verificação de integridade de arquivos.
- Análise inicial de arquivos suspeitos.
- Comparação de evidências digitais.
- Monitoramento de alterações em arquivos importantes.

Entretanto, encontrar hashes diferentes não revela automaticamente quem modificou o arquivo ou por qual motivo.

Também não significa que o arquivo seja malicioso.

Essas conclusões dependem de outras evidências e do contexto da investigação.

---

## Conclusão

Esta prática me ajudou a compreender de forma mais concreta como o SHA-256 pode ser utilizado para verificar a integridade de arquivos.

Consegui observar que uma alteração simples no conteúdo de um documento foi suficiente para produzir um hash diferente.

Além disso, utilizei comandos básicos do PowerShell para automatizar a comparação entre os valores.

Considero esse conhecimento relevante para minha preparação em segurança defensiva, principalmente por sua aplicação na análise de arquivos e na preservação de evidências durante investigações.

**[Voltar para Fundamentos de Criptografia](./fundamentos-criptografia.md)**
