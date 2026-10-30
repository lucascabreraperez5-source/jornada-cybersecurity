# Fundamentos de Redes — Protocolos, Portas e Análise de Tráfego

## Sobre o projeto

Este estudo faz parte do Módulo 5 da trilha **Cyber Security 101**, na plataforma TryHackMe.

O objetivo foi compreender os principais fundamentos de redes de computadores e entender como os dispositivos se comunicam, como os dados trafegam entre diferentes sistemas e qual é o papel dos protocolos nesse processo.

Esse conhecimento é especialmente importante em Cibersegurança, pois a análise de conexões, endereços IP, portas e tráfego de rede faz parte da rotina de investigação e identificação de possíveis ameaças.

## O que estudei

Ao longo das atividades, explorei os principais conceitos de redes e tive contato com ferramentas utilizadas para analisar e identificar informações sobre o tráfego e os dispositivos conectados.

Os assuntos estudados foram:

- Conceitos fundamentais de redes e modelos de comunicação OSI e TCP/IP.
- Protocolos de rede e diferenças entre TCP e UDP.
- Endereçamento IPv4, IPv6 e faixas de IP privadas.
- Portas de comunicação e seus respectivos serviços.
- Protocolos seguros e inseguros.
- Principais ataques relacionados à infraestrutura de redes.
- Sistemas de detecção e prevenção de intrusões (IDS e IPS).
- Análise de tráfego com Wireshark e tcpdump.
- Reconhecimento de hosts, portas e serviços utilizando Nmap.

## Conceitos importantes

### 1. Modelos OSI e TCP/IP

Estudei os modelos OSI e TCP/IP, utilizados para compreender como a comunicação entre dispositivos acontece em uma rede.

O modelo OSI divide essa comunicação em sete camadas, cada uma com uma função específica:

| Camada | Nome | Função |
|---|---|---|
| 7 | Aplicação | Comunicação entre aplicações e serviços, como HTTP e DNS. |
| 6 | Apresentação | Representação, codificação e transformação dos dados. |
| 5 | Sessão | Estabelecimento e gerenciamento de sessões de comunicação. |
| 4 | Transporte | Comunicação entre dispositivos por meio de TCP e UDP. |
| 3 | Rede | Endereçamento IP e roteamento de pacotes. |
| 2 | Enlace | Comunicação dentro da rede local, utilizando endereços MAC. |
| 1 | Física | Transmissão de dados por meios físicos. |

Também conheci o modelo TCP/IP, organizado em quatro camadas: Aplicação, Transporte, Internet e Acesso à Rede.

Compreender esses modelos ajuda a identificar em qual etapa da comunicação um problema ou uma atividade suspeita pode estar acontecendo.

### 2. TCP e UDP

Um dos pontos importantes do módulo foi entender as diferenças entre os dois principais protocolos de transporte.

| Característica | TCP | UDP |
|---|---|---|
| Comunicação | Orientada à conexão | Sem conexão |
| Confirmação de entrega | Sim | Não |
| Ordenação dos dados | Garante a sequência | Não garante |
| Handshake | Utiliza o 3-way handshake | Não utiliza |
| Exemplos | HTTP, HTTPS, SSH | DNS, VoIP e streaming |

O TCP prioriza a confiabilidade da comunicação, enquanto o UDP oferece uma comunicação mais simples, sem os mesmos mecanismos de confirmação e controle de entrega.

Também estudei como essas características podem ser exploradas em ataques, como SYN flood e ataques de amplificação UDP.

### 3. TCP 3-Way Handshake

O 3-way handshake é o processo utilizado pelo TCP para estabelecer uma conexão entre dois dispositivos.

Ele acontece em três etapas:

```text
Cliente              Servidor

   SYN  ------------>
        <------------ SYN-ACK
   ACK  ------------>

      Conexão estabelecida
```

- **SYN:** o cliente solicita o início de uma conexão.
- **SYN-ACK:** o servidor responde confirmando o recebimento e sinalizando que está pronto.
- **ACK:** o cliente confirma a resposta e a conexão é estabelecida.

Esse processo também está relacionado à segurança de redes. Em um ataque conhecido como **SYN flood**, um invasor envia um grande volume de solicitações SYN, tentando sobrecarregar o servidor com conexões que não chegam a ser completadas.

### 4. Endereçamento IP

Estudei como os endereços IP identificam dispositivos em uma rede e como são utilizados na comunicação entre sistemas.

Também conheci as principais faixas de endereços IPv4 privados:

| Faixa | Endereços |
|---|---|
| Classe A privada | `10.0.0.0/8` |
| Classe B privada | `172.16.0.0/12` |
| Classe C privada | `192.168.0.0/16` |

Outro conceito importante foi o CIDR, utilizado para representar máscaras de sub-rede.

Por exemplo, uma rede `192.168.1.0/24` possui 256 endereços IPv4 no total, normalmente com 254 endereços disponíveis para dispositivos.

Também estudei o IPv6, que utiliza endereços de 128 bits e amplia significativamente a quantidade de endereços disponíveis.

Na análise de segurança, diferenciar endereços privados e públicos ajuda a compreender a origem e o destino das conexões e a identificar possíveis comunicações suspeitas.

### 5. Portas e protocolos de rede

As portas permitem identificar quais serviços estão recebendo ou realizando conexões em um dispositivo.

Durante o módulo, estudei os serviços associados a algumas das portas mais utilizadas e sua importância para a segurança.

| Porta | Protocolo | Utilização | Relevância para segurança |
|---|---|---|---|
| **22** | SSH | Acesso remoto seguro | Tentativas de força bruta e acessos indevidos. |
| **53** | DNS | Resolução de nomes | DNS tunneling e outras atividades suspeitas. |
| **80** | HTTP | Comunicação web | Tráfego sem criptografia. |
| **443** | HTTPS | Comunicação web segura | Tráfego criptografado e possíveis comunicações maliciosas. |
| **445** | SMB | Compartilhamento de arquivos Windows | Ransomware e movimentação lateral. |
| **3389** | RDP | Acesso remoto ao Windows | Tentativas de invasão e acessos não autorizados. |
| 21 | FTP | Transferência de arquivos | Exposição de credenciais e dados. |
| 23 | Telnet | Acesso remoto | Comunicação sem criptografia. |
| 25 | SMTP | Envio de e-mails | Spam e abuso de servidores de e-mail. |
| 110 / 143 | POP3 / IMAP | Recebimento de e-mails | Possíveis problemas de autenticação e acesso. |
| 3306 | MySQL | Banco de dados | Exposição indevida de serviços de banco de dados. |

Entender essas portas é importante durante uma investigação, pois permite reconhecer quais serviços estão envolvidos em uma comunicação e avaliar se aquela conexão faz sentido dentro do ambiente analisado.

### 6. Protocolos seguros e inseguros

Também estudei a diferença entre protocolos que transmitem informações sem criptografia e alternativas que oferecem mecanismos de proteção.

| Protocolo | Alternativa | Diferença |
|---|---|---|
| HTTP | HTTPS | HTTPS utiliza TLS para proteger a comunicação. |
| FTP | SFTP / FTPS | Alternativas que oferecem proteção na transferência de arquivos. |
| Telnet | SSH | SSH oferece comunicação criptografada para acesso remoto. |
| DNS tradicional | DNS over HTTPS (DoH) | DoH protege as consultas DNS utilizando HTTPS. |

Esse conhecimento ajuda a identificar situações em que informações sensíveis podem estar expostas durante a comunicação entre dispositivos.

### 7. Principais ataques de rede

Durante os estudos, conheci diferentes técnicas utilizadas para explorar vulnerabilidades em protocolos e serviços de rede.

Entre elas:

- **ARP Spoofing:** manipulação de mensagens ARP para associar um endereço IP ao endereço MAC de um dispositivo controlado pelo atacante, podendo possibilitar ataques Man-in-the-Middle.
- **DNS Cache Poisoning:** manipulação de informações armazenadas no cache DNS para direcionar usuários a endereços incorretos.
- **DNS Tunneling:** utilização de consultas e respostas DNS para transportar dados de maneira disfarçada.
- **DNS Amplification:** ataque de negação de serviço que utiliza respostas DNS amplificadas para gerar grande volume de tráfego contra uma vítima.
- **SYN Flood:** envio excessivo de solicitações de conexão TCP para sobrecarregar um serviço.

Conhecer essas técnicas ajuda a compreender quais comportamentos podem indicar atividades maliciosas durante uma análise de tráfego.

### 8. IDS, IPS e segmentação de rede

Também estudei alguns mecanismos utilizados para proteger e monitorar ambientes de rede.

- **IDS (Intrusion Detection System):** monitora o tráfego e identifica possíveis atividades suspeitas, gerando alertas.
- **IPS (Intrusion Prevention System):** além de identificar ameaças, pode atuar bloqueando tráfego considerado malicioso.
- **Segmentação de rede:** divide uma infraestrutura em diferentes segmentos, limitando a comunicação entre eles e reduzindo a possibilidade de propagação de ameaças.

Esses mecanismos são importantes para identificar atividades suspeitas e limitar os impactos de possíveis comprometimentos.

### 9. Ferramentas de análise de rede

Durante o módulo, tive contato com três ferramentas importantes para análise e reconhecimento de redes.

| Ferramenta | Finalidade | Aplicação |
|---|---|---|
| **Wireshark** | Captura e análise de pacotes por interface gráfica. | Inspecionar comunicações e identificar comportamentos suspeitos. |
| **tcpdump** | Captura e análise de tráfego pela linha de comando. | Monitorar conexões e capturar pacotes em servidores. |
| **Nmap** | Descoberta de hosts, portas abertas e serviços. | Identificar serviços disponíveis e possíveis exposições na rede. |

Também revisei comandos úteis para consultar configurações e informações de rede:

- `ipconfig` / `ip addr`: consultar configurações de rede.
- `nslookup`: consultar informações de resolução DNS.
- `tracert` / `traceroute`: visualizar o caminho percorrido até um destino.
- `netstat -ano` / `ss -tulnp`: consultar conexões e portas utilizadas pelo sistema.

## Principais aprendizados

Esse módulo ampliou bastante minha compreensão sobre como funciona a comunicação entre dispositivos e como os diferentes protocolos e serviços participam desse processo.

Um dos pontos mais importantes foi entender que informações como endereço IP, porta, protocolo e estado de uma conexão ajudam a construir uma visão muito mais clara sobre o que está acontecendo em uma rede.

Também comecei a perceber como determinados comportamentos podem indicar possíveis ameaças. Uma quantidade incomum de conexões, tentativas repetidas de acesso ou comunicações inesperadas com serviços externos são exemplos de situações que podem merecer investigação.

Outro aprendizado importante foi conhecer ferramentas como Wireshark, tcpdump e Nmap, que permitem sair da teoria e observar informações reais sobre uma infraestrutura de rede.

## Aplicação em Cibersegurança

O conhecimento de redes está diretamente relacionado às atividades de um analista de **Segurança Defensiva (Blue Team)**, principalmente em ambientes SOC.

Entre suas aplicações estão:

- Analisar endereços IP, portas e protocolos envolvidos em alertas.
- Investigar conexões suspeitas entre dispositivos internos e externos.
- Identificar serviços expostos e possíveis pontos de entrada.
- Analisar pacotes para compreender o comportamento de uma comunicação.
- Reconhecer padrões associados a ataques como SYN flood, DNS tunneling e ARP spoofing.
- Compreender regras de firewall e mecanismos de segmentação de rede.

Esses fundamentos também serão importantes nos meus próximos estudos sobre monitoramento, ferramentas SIEM e segurança em ambientes de nuvem.

## Próximos passos

Aprofundar a utilização do Wireshark e tcpdump, praticar a identificação de diferentes protocolos e realizar novas atividades de reconhecimento com Nmap.

Como próximo exercício, pretendo capturar e analisar o tráfego gerado ao acessar um site, acompanhando as etapas de resolução DNS, estabelecimento de conexão TCP e comunicação HTTPS.
