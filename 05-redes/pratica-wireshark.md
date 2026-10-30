# Prática — Análise de Tráfego de Rede com Wireshark

**Módulo:** Fundamentos de Redes — TryHackMe Cyber Security 101

## Objetivo

Colocar em prática os conceitos estudados no módulo de redes, utilizando o Wireshark para observar como acontecem a resolução DNS, o estabelecimento de uma conexão TCP e a comunicação criptografada com TLS.

## Ambiente

- **Sistema:** Windows 11
- **Ferramenta:** Wireshark
- **Interface:** Ethernet 2
- **Tráfego analisado:** Google (DNS) e GitHub API (TCP/TLS)

## 1. Resolução DNS

Comecei capturando o tráfego de rede e utilizei o filtro abaixo para localizar as consultas relacionadas ao Google:

```text
dns.qry.name == "google.com"
```

Identifiquei uma consulta DNS do tipo A e a resposta retornando o endereço IP `142.250.79.206`.

<img width="1425" height="893" alt="01-dns-google" src="https://github.com/user-attachments/assets/c65e97b7-7f7f-43ee-bc60-d5ec142c7b1b" />

Com isso, consegui observar como o computador consulta um servidor DNS para descobrir o endereço IP associado a um domínio.

## 2. Estabelecimento da conexão TCP

Para identificar conexões iniciadas pelo meu computador, utilizei:

```text
ip.src == 192.168.0.105 && tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Depois, isolei uma conexão com o endereço `4.228.31.149`, na porta 443:

```text
ip.addr == 4.228.31.149 && tcp.port == 60614
```

Encontrei as três etapas do **3-Way Handshake**:

1. SYN — meu computador solicita a conexão.
2. SYN-ACK — o servidor responde.
3. ACK — meu computador confirma.

<img width="1435" height="888" alt="02-handshake-tcp" src="https://github.com/user-attachments/assets/b7f422e7-4f13-4e37-8df9-0f82ba0958cf" />

Essa etapa me ajudou a visualizar como uma conexão TCP é estabelecida antes da comunicação da aplicação.

## 3. Comunicação criptografada com TLS

Na mesma conexão, utilizei o filtro:

```text
ip.addr == 4.228.31.149 && tcp.port == 60614 && tls
```

Identifiquei as mensagens **Client Hello** e **Server Hello**, relacionadas à negociação TLS.

Também observei que a conexão utilizou TLS 1.3 e a Cipher Suite `TLS_AES_128_GCM_SHA256`.

<img width="1435" height="892" alt="03-tls-server-hello" src="https://github.com/user-attachments/assets/1090f1ed-a96d-4f44-9adc-c1a56b73b529" />

Após a negociação, encontrei pacotes classificados como **Application Data**, cujo conteúdo estava criptografado.

<img width="1436" height="893" alt="04-tls-dados-criptografados" src="https://github.com/user-attachments/assets/b1d2932f-e57c-412f-ae06-e53d1cf1620b" />

Entendi que, mesmo sem conseguir ler o conteúdo da comunicação, ainda é possível analisar informações como IPs, portas, protocolos e tamanho dos pacotes.

## Descoberta durante a prática

Ao procurar conexões TCP relacionadas ao Google, percebi que nem todo tráfego HTTPS utiliza TCP.

Também encontrei pacotes QUIC, protocolo baseado em UDP e utilizado pelo HTTP/3. Não confirmei se esses pacotes pertenciam ao acesso ao Google.

Por isso, utilizei uma conexão TCP/TLS relacionada ao `api.github.com` para continuar a análise.

## Conclusão

Essa prática me ajudou a conectar a teoria com o funcionamento real das redes.

Aprendi a utilizar filtros no Wireshark, identificar consultas DNS, reconhecer o handshake TCP e observar uma comunicação protegida por TLS.

Também percebi como essas informações podem ser úteis em investigações de segurança, principalmente para identificar conexões suspeitas e compreender o comportamento do tráfego em ambientes SOC.
