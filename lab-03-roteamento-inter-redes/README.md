# 📍 Lab 03: Roteamento entre Duas Redes Lógicas Distintas

## 1. Descrição e Objetivo
O objetivo deste laboratório é demonstrar o papel fundamental do roteador na interconexão e no encaminhamento de pacotes entre dois segmentos de redes IPv4 distintos (Layer 3), validando o uso correto de gateways padrão para comunicação fim a fim[cite: 5].

---

## 2. Diagrama de Topologia
<img width="1010" height="368" alt="image" src="https://github.com/user-attachments/assets/cad2e892-4cc6-4039-8215-218d9d67012c" />

---

## 3. Tabela de Endereçamento
| Dispositivo | Interface | Endereço IP | Máscara de Sub-rede | Gateway Padrão | Rede de Origem |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Router0** | Gig0/0 | 192.168.1.1 | 255.255.255.0 (/24) | N/A | Rede 1 (Gateway)[cite: 5] |
| **Router0** | Gig0/1 | 192.168.2.1 | 255.255.255.0 (/24) | N/A | Rede 2 (Gateway)[cite: 5] |
| **PC0** | FastEthernet0 | 192.168.1.10 | 255.255.255.0 (/24) | 192.168.1.1 | Rede 1[cite: 5] |
| **PC1** | FastEthernet0 | 192.168.2.10 | 255.255.255.0 (/24) | 192.168.2.1 | Rede 2[cite: 5] |

---

## 4. Comandos e Configuração CLI (Roteador)

    ! Acessando o modo de configuração global
    Router> enable
    Router# configure terminal

    ! Configuração da Interface Gig0/0 (Gateway da Rede 1)
    Router(config)# interface gigabitethernet 0/0
    Router(config-if)# ip address 192.168.1.1 255.255.255.0
    Router(config-if)# no shutdown
    Router(config-if)# exit

    ! Configuração da Interface Gig0/1 (Gateway da Rede 2)
    Router(config)# interface gigabitethernet 0/1
    Router(config-if)# ip address 192.168.2.1 255.255.255.0
    Router(config-if)# no shutdown
    Router(config-if)# exit

---

## 5. Troubleshooting e Metodologia Aplicada
* **Causa Raiz Identificada:** Durante a configuração inicial, foram atribuídos os endereços de identificação de rede (`192.168.1.0` e `192.168.2.0`) nas interfaces do roteador em vez de endereços de host utilizáveis, impedindo a comunicação dos pacotes ICMP[cite: 5].
* **Correção Executada:** Ajustou-se o endereçamento das interfaces para os primeiros IPs válidos de cada bloco (`192.168.1.1` e `192.168.2.1`), configurando-os devidamente como os Default Gateways dos hosts `PC0` e `PC1`[cite: 5].

---

## 6. Validação e Resultados
* **Teste de Ping Inter-Redes:**
  * Disparado do `PC0` (`192.168.1.10`) em direção ao `PC1` (`192.168.2.10`)[cite: 5].
  * Comunicação estabelecida com sucesso através do roteamento entre interfaces do `Router0`[cite: 5].

---

## 7. Análise Técnica
* **Por que dois switches isolados não se comunicam sem o roteador?**
  > Os switches operam na Camada 2 (Enlace) e não realizam encaminhamento entre sub-redes lógicas diferentes. Quando o `PC0` identifica que o destino (`192.168.2.10`) está fora de sua sub-rede local (`192.168.1.0/24`), ele obrigatoriamente encapsula o pacote com o endereço MAC de destino do seu Gateway Padrão (Roteador), permitindo a travessia de camada 3[cite: 5].
