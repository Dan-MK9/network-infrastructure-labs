# 📍 Lab 02: Configuração de Servidor DHCP em Roteador

## 1. Descrição e Objetivo
O objetivo deste laboratório é configurar um roteador Cisco para atuar como servidor DHCP (Dynamic Host Configuration Protocol), permitindo a alocação dinâmica e automática de endereços IPv4, máscara de sub-rede, gateway padrão e servidor DNS para os hosts da rede local.

---

## 2. Diagrama de Topologia
<img width="912" height="349" alt="image" src="https://github.com/user-attachments/assets/84cc54a5-14c3-4429-8ae2-40b6f15da88a" />

---

## 3. Tabela de Endereçamento e Parâmetros DHCP
| Parâmetro | Valor Configurado | Descrição |
| :--- | :--- | :--- |
| **Rede (Pool)** | 192.168.10.0/24 | Sub-rede atendida pelo serviço DHCP |
| **Gateway Padrão (Router)** | 192.168.10.1 (Gig0/0) | Interface do roteador e saída padrão dos hosts |
| **Faixa de IPs Excluídos** | 192.168.1.1 até 192.168.1.10 | Reserva estática para infraestrutura/servidores |
| **Servidor DNS** | 8.8.8.8 | Servidor DNS entregue via lease aos clientes |
| **Host (PC0)** | Atribuição via DHCP | IP obtido dinamicamente (192.168.10.2) |

---

## 4. Comandos e Configuração CLI (Roteador)

    ! Acessando o modo de configuração global
    Router> enable
    Router# configure terminal

    ! Configuração da interface local (Gateway)
    Router(config)# interface gigabitethernet 0/0
    Router(config-if)# ip address 192.168.10.1 255.255.255.0
    Router(config-if)# no shutdown
    Router(config-if)# exit

    ! Reserva de endereços estáticos (fora do escopo de concessão)
    Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10

    ! Criação e parametrização do Pool DHCP
    Router(config)# ip dhcp pool RedeLab2
    Router(dhcp-config)# network 192.168.10.0 255.255.255.0
    Router(dhcp-config)# default-router 192.168.10.1
    Router(dhcp-config)# dns-server 8.8.8.8
    Router(dhcp-config)# exit

---

## 5. Validação e Testes
1. No host cliente (`PC0`), alternou-se a configuração de rede de **Static** para **DHCP**.
2. O processo de negociação (DORA) foi concluído com sucesso, exibindo a mensagem: `DHCP request successful`.
3. O host recebeu os seguintes parâmetros:
   * **IP Address:** `192.168.10.2`
   * **Subnet Mask:** `255.255.255.0`
   * **Default Gateway:** `192.168.10.1`
   * **DNS Server:** `8.8.8.8`

---

## 6. Análise Técnica
* **Como o host localiza o servidor DHCP na rede?**
  > Quando configurado em modo DHCP, o cliente envia inicialmente uma mensagem de broadcast camada 2 e camada 3 (*DHCP Discover*). O roteador conectado ao segmento escuta a requisição na porta UDP 67 e responde oferecendo um endereço disponível (*DHCP Offer*), iniciando o ciclo DORA.
* **Qual é a importância da exclusão de endereços (`ip dhcp excluded-address`)?**
  > Impede que o pool DHCP entregue automaticamente endereços IP que devem permanecer fixos/estáticos na infraestrutura (como interfaces de roteadores, switches gerenciáveis, servidores ou impressoras de rede), evitando conflitos de IP duplicado.
