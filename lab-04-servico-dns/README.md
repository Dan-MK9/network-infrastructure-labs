# 📍 Lab 04: Resolução de Nomes com Servidor DNS

## 1. Descrição e Objetivo
O objetivo deste laboratório é demonstrar o funcionamento do serviço de resolução de nomes (DNS - Domain Name System), configurando um servidor dedicado para mapear um nome de domínio local (FQDN) para seu respectivo endereço IPv4 e validando o acesso via navegador web a partir de um host cliente.

---

## 2. Diagrama de Topologia
 <img width="751" height="333" alt="topology" src="https://github.com/user-attachments/assets/78284e40-29a7-4c23-8437-9fa87b84f9a7" />

---

## 3. Tabela de Endereçamento e Conexões
| Dispositivo | Modelo | Interface | Conectado a | Endereço IP | Máscara de Sub-rede | Gateway Padrão | Servidor DNS |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **PC0** | PC-PT | Fa0 | Switch0 (Fa0/1) | Atribuído na rede | 255.255.255.0 (/24) | N/A | 192.168.1.100 |
| **Switch0** | 2960-24TT | Fa0/1 | PC0 (Fa0) | Layer 2 Switch | N/A | N/A | N/A |
| **Switch0** | 2960-24TT | Fa0/2 | Server-DNS (Fa0) | Layer 2 Switch | N/A | N/A | N/A |
| **Server-DNS** | Server-PT | Fa0 | Switch0 (Fa0/2) | 192.168.1.100 | 255.255.255.0 (/24) | N/A | 127.0.0.1 / Local |

---

## 4. Configuração dos Serviços (Server-DNS)

    ! Parâmetros de Rede do Servidor
    Endereço IP: 192.168.1.100
    Máscara de Sub-rede: 255.255.255.0

    ! Configuração do Serviço DNS (Aba Services > DNS)
    Status do Serviço: ON
    Nome do Registro (Name): www.empresa.local
    Tipo de Registro: A Record (Address)
    Endereço Mapeado (Address): 192.168.1.100

---

## 5. Metodologia e Passos Realizados
1. **Montagem e Conectividade:** Conexão do `PC0` e do `Server-DNS` nas portas FastEthernet do switch `2960-24TT`.
2. **Parametrização do Servidor:** Definição do endereço IP estático `192.168.1.100` e criação da entrada DNS apontando o domínio `www.empresa.local` para o endereço do servidor.
3. **Configuração do Cliente:** Inserção manual do endereço `192.168.1.100` no campo **DNS Server** do `PC0`.
4. **Validação:** Abertura do navegador web (Web Browser) no `PC0` e inserção da URL `http://www.empresa.local`.

---

## 6. Validação e Resultados
* **Acesso HTTP via Nome:** A página web padrão foi carregada com êxito após o navegador resolver o nome `www.empresa.local` para o IP `192.168.1.100` via consulta DNS.

---

## 7. Análise Técnica
* **Como o cliente descobre o endereço IP através do DNS?**
  > O host envia uma requisição DNS (DNS Query via UDP na porta 53) para o endereço de servidor DNS cadastrado em sua interface de rede. O servidor consulta sua tabela de registros locais (ou encaminha a requisição caso não possua o registro) e retorna uma resposta com o endereço IP de destino mapeado.
* **Qual seria o impacto na rede sem o serviço de DNS?**
  > Os usuários precisariam digitar diretamente o endereço IP numérico de cada destino (ex: `172.217.172.16`) para estabelecer conexões com servidores e aplicações web, inviabilizando a usabilidade e a escalabilidade dos serviços.
