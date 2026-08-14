# 📍 Lab 03: Roteamento entre Duas Redes Lógicas Distintas

## 1. Descrição e Objetivo
O objetivo deste laboratório é demonstrar o papel fundamental do roteador na interconexão e no encaminhamento de pacotes entre dois segmentos de redes IPv4 distintos (Layer 3), validando o uso correto de gateways padrão para comunicação fim a fim.

---

## 2. Diagrama de Topologia
<img width="1014" height="369" alt="image" src="https://github.com/user-attachments/assets/63c94c0d-4a52-4a01-b8a7-7bd96f77a033" />

---

## 3. Tabela de Endereçamento e Conexões
| Dispositivo | Modelo | Interface | Conectado a | Endereço IP | Máscara de Sub-rede | Gateway Padrão | Segmento de Rede |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **PC4** | PC-PT | Fa0 | Switch0 (Fa0/2) | 192.168.1.10 | 255.255.255.0 (/24) | 192.168.1.1 | Rede |
| **Switch0** | 2950T-24 | Fa0/1 | Router2 (Gig0/0) | Layer 2 Switch | N/A | N/A | Rede 1 |
| **Router2** | Cisco 2911 | Gig0/0 | Switch0 (Fa0/1) | 192.168.1.1 | 255.255.255.0 (/24) | N/A | Rede 1 (Gateway) |
| **Router2** | Cisco 2911 | Gig0/1 | Switch1 (Fa0/1) | 192.168.2.1 | 255.255.255.0 (/24) | N/A | Rede 2 (Gateway) |
| **Switch1** | 2950T-24 | Fa0/1 | Router2 (Gig0/1) | Layer 2 Switch | N/A | N/A | Rede 2 |
| **PC5** | PC-PT | Fa0 | Switch1 (Fa0/2) | 192.168.2.10 | 255.255.255.0 (/24) | 192.168.2.1 | Rede 2 |

---

## 4. Comandos e Configuração CLI (Router2)

    ! Acessando o modo de configuração global
    Router2> enable
    Router2# configure terminal

    ! Configuração da Interface Gig0/0 (Gateway da Rede 1)
    Router2(config)# interface gigabitethernet 0/0
    Router2(config-if)# ip address 192.168.1.1 255.255.255.0
    Router2(config-if)# no shutdown
    Router2(config-if)# exit

    ! Configuração da Interface Gig0/1 (Gateway da Rede 2)
    Router2(config)# interface gigabitethernet 0/1
    Router2(config-if)# ip address 192.168.2.1 255.255.255.0
    Router2(config-if)# no shutdown
    Router2(config-if)# exit

---

## 5. Troubleshooting e Metodologia Aplicada
* **Causa Raiz Identificada:** Durante a configuração inicial, foram atribuídos os endereços de identificação de rede (`192.168.1.0` e `192.168.2.0`) nas interfaces do roteador em vez de endereços de host utilizáveis, gerando erro de atribuição e impedindo o tráfego dos pacotes ICMP.
* **Correção Executada:** Ajustou-se o endereçamento das interfaces para os primeiros IPs utilizáveis de cada bloco (`192.168.1.1` e `192.168.2.1`), configurando-os devidamente como os Default Gateways dos hosts `PC4` e `PC5`.

---

## 6. Validação e Resultados
* **Teste de Ping Inter-Redes:**
  * Disparado do `PC4` (`192.168.1.10`) com destino ao `PC5` (`192.168.2.10`).
  * Comunicação estabelecida com sucesso através do roteamento direto entre as sub-redes conectadas ao `Router2`.

---

## 7. Análise Técnica
* **Por que dois switches isolados não se comunicam sem o roteador?**
  > Os switches operam na Camada 2 (Enlace) e não realizam encaminhamento entre sub-redes lógicas diferentes. Quando o `PC4` identifica que o destino (`192.168.2.10`) está fora de sua sub-rede local (`192.168.1.0/24`), ele obrigatoriamente encapsula o pacote com o endereço MAC de destino do seu Gateway Padrão (`Router2`), permitindo a travessia de camada 3.
