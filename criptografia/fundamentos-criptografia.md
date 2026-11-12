# Fundamentos de Criptografia — Hashes e Integridade de Arquivos

> **Criptografia e Segurança da Informação**

---

## Sobre o projeto

Neste módulo, procurei compreender como diferentes técnicas ajudam a proteger dados, verificar a integridade de arquivos e identificar alterações em informações.

Também realizei uma atividade prática utilizando o **Windows PowerShell**, na qual calculei o hash SHA-256 de um arquivo, modifiquei seu conteúdo e comparei os resultados para identificar a alteração.

O objetivo foi relacionar os conceitos teóricos a uma situação simples que pode ser aplicada em investigações de segurança.

## O que estudei

- Conceitos fundamentais de criptografia.
- Confidencialidade, integridade e autenticidade.
- Diferença entre criptografia simétrica e assimétrica.
- Funções hash e suas características.
- Funcionamento do SHA-256.
- Diferença entre hashing e criptografia.
- Aplicações de hashes na segurança defensiva.
- Verificação de integridade com PowerShell.

---

## 1. O que é criptografia?

A criptografia reúne técnicas utilizadas para proteger informações, impedindo que pessoas não autorizadas consigam compreender determinados dados.

Ela está presente em diversas atividades do dia a dia, como conexões HTTPS, comunicação entre sistemas, armazenamento de informações e autenticação.

Um exemplo é o acesso a um site por HTTPS. Nesse caso, protocolos de segurança como o TLS utilizam mecanismos criptográficos para proteger a comunicação entre o navegador e o servidor.

### Objetivos da segurança da informação

| Conceito | Significado |
|---|---|
| Confidencialidade | Garantir que somente pessoas autorizadas tenham acesso às informações |
| Integridade | Garantir que alterações nos dados possam ser identificadas |
| Disponibilidade | Manter sistemas e informações acessíveis quando necessários |
| Autenticidade | Verificar a identidade ou a origem de uma informação |

**O que entendi:** criptografia não serve apenas para esconder informações. Diferentes mecanismos criptográficos também podem ajudar a verificar a autenticidade e a integridade dos dados.

---

## 2. Criptografia simétrica e assimétrica

Existem diferentes métodos de criptografia, utilizados conforme a necessidade de proteção.

### Criptografia simétrica

Na criptografia simétrica, a mesma chave secreta é utilizada para criptografar e descriptografar os dados.

Um exemplo conhecido é o **AES (Advanced Encryption Standard)**.

Esse método é bastante utilizado na proteção de arquivos, discos e comunicações, principalmente por sua eficiência.

O principal cuidado está no gerenciamento da chave: se ela for comprometida, a proteção dos dados também poderá ser afetada.

### Criptografia assimétrica

Na criptografia assimétrica, são utilizadas duas chaves relacionadas matematicamente:

- **Chave pública:** pode ser compartilhada.
- **Chave privada:** deve permanecer protegida.

Essas chaves podem ser utilizadas em operações como criptografia e assinatura digital, dependendo do algoritmo.

Entre os exemplos estão RSA e algoritmos de criptografia de curva elíptica.

### Comparação

| Característica | Simétrica | Assimétrica |
|---|---|---|
| Chaves | Uma chave secreta compartilhada | Par de chaves pública e privada |
| Desempenho | Geralmente mais rápida | Geralmente mais custosa computacionalmente |
| Exemplo | AES | RSA |
| Aplicação | Proteção de grandes volumes de dados | Troca de chaves, criptografia e assinaturas digitais |

Na prática, sistemas modernos podem combinar os dois métodos, aproveitando as características de cada um.

---

## 3. O que é uma função hash?

Uma função hash transforma uma entrada, como um texto ou arquivo, em um valor de tamanho definido.

Esse resultado é chamado de **hash** ou resumo criptográfico.

Diferentemente da criptografia, uma função hash criptográfica não foi projetada para permitir que o conteúdo original seja recuperado a partir do resultado.

### Características importantes

- **Determinística:** a mesma entrada produz o mesmo hash.
- **Tamanho fixo:** o resultado possui tamanho definido pelo algoritmo.
- **Sensível a alterações:** pequenas modificações geralmente produzem resultados bastante diferentes.
- **Resistência à pré-imagem:** deve ser computacionalmente difícil recuperar uma entrada a partir de seu hash.
- **Resistência a colisões:** deve ser difícil encontrar duas entradas diferentes que produzam o mesmo hash.

É importante lembrar que hashes não são identificadores matematicamente exclusivos: colisões são possíveis, embora algoritmos modernos sejam projetados para torná-las extremamente difíceis de encontrar.

### Hashing não é criptografia reversível

| Hashing | Criptografia |
|---|---|
| Produz um resumo dos dados | Transforma dados para protegê-los |
| Não utiliza uma operação de descriptografia | Permite recuperar os dados com a chave adequada |
| Pode ajudar a verificar integridade | Pode proteger a confidencialidade |
| Exemplo: SHA-256 | Exemplo: AES |

---

## 4. SHA-256

O **SHA-256 (Secure Hash Algorithm 256-bit)** é uma função hash criptográfica da família SHA-2.

Independentemente do tamanho do arquivo analisado, o resultado SHA-256 possui **256 bits**, normalmente representados por 64 caracteres hexadecimais.

Ele pode ser utilizado para:

- Verificar a integridade de arquivos.
- Comparar arquivos com versões de referência.
- Identificar arquivos por meio de seus hashes durante investigações.
- Apoiar processos de verificação de downloads e evidências digitais.

### Exemplo de utilização

Imagine que um arquivo seja baixado de uma fonte confiável.

Se o fornecedor disponibilizar um hash SHA-256 autêntico, podemos calcular o hash do arquivo baixado e comparar os valores.

**Hashes iguais:** indicam que os conteúdos correspondem segundo a verificação realizada.

**Hashes diferentes:** indicam que os conteúdos não correspondem, podendo ter ocorrido uma alteração, corrupção ou substituição.

Entretanto, um hash sozinho não comprova que um arquivo é seguro.

Um arquivo malicioso também possui um hash válido. A integridade depende da comparação com uma referência confiável.

---

## 5. Integridade de arquivos

A integridade está relacionada à capacidade de identificar alterações em dados.

Em segurança defensiva, essa verificação pode ajudar a descobrir se arquivos foram modificados durante uma transferência, armazenamento ou investigação.

### Exemplo de alteração

Considere um arquivo de texto com o seguinte conteúdo:

`Arquivo original para análise de integridade`

Após calcular seu SHA-256, adicionamos uma nova linha:

`Alteracao realizada durante a investigacao`

Quando o hash é calculado novamente, o resultado muda porque o conteúdo do arquivo foi modificado.

Esse comportamento permite detectar diferenças sem precisar comparar manualmente todo o conteúdo.

### Limitações

A comparação de hashes precisa ser interpretada corretamente:

- Um hash diferente confirma que os conteúdos comparados não são iguais.
- Um hash igual não prova que o arquivo seja legítimo ou seguro.
- O hash de referência precisa ter sido obtido de uma fonte confiável.
- A comparação não identifica, sozinha, quem alterou o arquivo nem por qual motivo.

---

## 6. Verificação de hashes no PowerShell

O Windows PowerShell possui o comando `Get-FileHash`, que permite calcular hashes de arquivos.

Na prática, utilizei o algoritmo SHA-256.

### Exemplo de comando

```powershell
Get-FileHash "$HOME\lab-integridade\evidencia.txt" -Algorithm SHA256
```

### Entendendo o comando

| Parte | Função |
|---|---|
| `Get-FileHash` | Calcula o hash de um arquivo |
| `$HOME` | Representa a pasta do usuário no Windows |
| `lab-integridade\evidencia.txt` | Caminho do arquivo analisado |
| `-Algorithm SHA256` | Define SHA-256 como algoritmo utilizado |

O resultado mostra o algoritmo, o hash calculado e o caminho do arquivo.

Durante o laboratório, executei o comando antes e depois de modificar o arquivo, comparando os valores para verificar se a integridade havia sido preservada.

---

## 7. Hashes na segurança defensiva

Hashes possuem diversas aplicações no trabalho de um analista de segurança.

### Análise de arquivos suspeitos

Durante uma investigação, o analista pode calcular o hash de um executável e utilizá-lo como indicador para procurar registros relacionados ao mesmo arquivo.

Serviços de inteligência de ameaças também podem disponibilizar informações associadas a hashes conhecidos.

Entretanto, a reputação de um hash não deve ser utilizada isoladamente para determinar se um arquivo é malicioso.

### Integridade de arquivos importantes

A comparação de hashes pode ajudar a identificar alterações em arquivos de configuração, documentos e componentes de sistemas.

Em ambientes corporativos, essa verificação pode fazer parte de mecanismos de **File Integrity Monitoring (FIM)**.

### Investigação e preservação de evidências

Durante uma análise forense, hashes podem ser utilizados para documentar a integridade de arquivos e cópias de evidências.

Isso ajuda a demonstrar que determinado conteúdo permaneceu igual entre etapas da investigação.

A utilização de hashes não substitui outros cuidados, como procedimentos de coleta, registros de acesso e cadeia de custódia.

---

## 8. Hashes e segurança de senhas

Outra aplicação importante das funções hash está na proteção de senhas.

Sistemas podem armazenar representações derivadas das senhas, evitando guardar diretamente o texto original.

Entretanto, **não é recomendado armazenar senhas utilizando apenas SHA-256 puro**.

Para essa finalidade, são utilizados algoritmos próprios para derivação de senhas, como:

- Argon2id
- bcrypt
- scrypt
- PBKDF2

Esses mecanismos normalmente utilizam salt e parâmetros que aumentam o custo computacional de tentativas de descoberta de senhas.

Ferramentas como o John the Ripper podem ser utilizadas em auditorias autorizadas para avaliar a resistência de senhas.

Neste projeto, porém, a atividade prática ficou concentrada na verificação de integridade de arquivos com SHA-256.

---

## 9. O que aprendi

Este módulo me ajudou a compreender melhor a diferença entre proteger o conteúdo de uma informação e verificar se ela foi modificada.

Os principais aprendizados foram:

- Identificar diferenças entre criptografia simétrica, assimétrica e hashing.
- Compreender o funcionamento básico do SHA-256.
- Calcular hashes de arquivos utilizando PowerShell.
- Detectar alterações por meio da comparação de valores.
- Reconhecer aplicações de hashes em segurança defensiva.
- Entender que a existência de um hash não comprova que um arquivo seja seguro.

A parte mais importante foi conseguir visualizar, na prática, como uma alteração simples no conteúdo produz um resultado diferente.

---

## 10. Aplicação em Cibersegurança

Para minha preparação como **Analista SOC Júnior**, considero importante conhecer hashes e seus usos em investigações.

Esses conhecimentos podem ser aplicados na análise de arquivos suspeitos, consulta de indicadores de comprometimento, verificação de integridade e documentação de evidências.

A prática também reforçou o uso do PowerShell, que já havia estudado anteriormente em atividades de linha de comando e triagem de endpoints.

---

## Atividade prática — Verificação de Integridade com SHA-256

Para aplicar os conceitos, criei um arquivo de teste no Windows e calculei seu hash SHA-256 utilizando o PowerShell.

Depois, modifiquei o conteúdo do arquivo, calculei novamente o hash e realizei uma comparação automática entre o valor original e o atual.

O resultado confirmou que os hashes eram diferentes, demonstrando a detecção de uma alteração no conteúdo.

Documentei os comandos utilizados e os resultados obtidos com capturas reais da atividade.

**[Ver atividade prática — Verificação de Integridade com SHA-256](./pratica-integridade-sha256.md)**
