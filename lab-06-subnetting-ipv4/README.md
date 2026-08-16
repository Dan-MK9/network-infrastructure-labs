# 📍 Lab 06: Planejamento e Divisão de Sub-redes IPv4 (Subnetting)

## 1. Descrição e Objetivo
O objetivo deste laboratório é dimensionar, calcular e implementar uma divisão de sub-redes IPv4 a partir de um bloco de classe C (`192.168.100.0/24`), atendendo ao requisito de escalabilidade para 3 departamentos (RH, Financeiro e TI com até 50 dispositivos cada) e garantindo a comunicação inter-redes através de um roteador central.

---

## 2. Diagrama de Topologia
<img width="1865" height="642" alt="topology" src="https://github.com/user-attachments/assets/834ebdb7-09aa-4939-ba41-b76c84dbfce9" />

---

## 3. Dimensionamento e Cálculo de Subnetting

* **Bloco Base Original:** `192.168.100.0/24` (256 endereços totais)
* **Requisito:** Mínimo de 50 dispositivos utilizáveis por setor
* **Cálculo da Máscara:** 
  * Potência de 2 necessária: $2^6 = 64$ endereços totais por sub-rede ($64 - 2 = 62$ hosts utilizáveis).
  * Máscara binária: `11111111.11111111.11111111.11000000` $\rightarrow$ Decimal: `255.255.255.192` (`/26`).

---

## 4. Tabela de Endereçamento e Sub-redes

| Departamento | ID de Rede | Primeiro Host | Último Host | Broadcast | Gateway Padrão (Router) | Máscara |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **RH** | `192.168.100.0/26` | `192.168.100.1` | `192.168.100.62` | `192.168.100.63` | `192.168.100.1` (Gig0/0/0) | `255.255.255.192` |
| **Financeiro** | `192.168.100.64/26` | `192.168.100.65` | `192.168.100.126` | `192.168.100.127` | `192.168.100.65` (Gig0/0/1) | `255.255.255.192` |
| **TI** | `192.168.100.128/26` | `192.168.100.129` | `192.168.100.190` | `192.168.100.191` | `192.168.100.129` (Gig0/0/2) | `255.255.255.192` |

---

## 5. Configuração CLI das Interfaces (Roteador)

    Router# configure terminal

    ! Gateway Sub-rede RH (Gig0/0/0)
    Router(config)# interface gigabitethernet 0/0/0
    Router(config-if)# ip address 192.168.100.1 255.255.255.192
    Router(config-if)# no shutdown
    Router(config-if)# exit

    ! Gateway Sub-rede Financeiro (Gig0/0/1)
    Router(config)# interface gigabitethernet 0/0/1
    Router(config-if)# ip address 192.168.100.65 255.255.255.192
    Router(config-if)# no shutdown
    Router(config-if)# exit

    ! Gateway Sub-rede TI (Gig0/0/2)
    Router(config)# interface gigabitethernet 0/0/2
    Router(config-if)# ip address 192.168.100.129 255.255.255.192
    Router(config-if)# no shutdown
    Router(config-if)# exit

---

## 6. Evidências de Validação e Testes

* **Teste 1: Conectividade Local com o Gateway (PC0 $\rightarrow$ Gateway RH `192.168.100.1`)**
```text
C:\>ping 192.168.100.1

Pinging 192.168.100.1 with 32 bytes of data:
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255
Reply from 192.168.100.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.100.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
