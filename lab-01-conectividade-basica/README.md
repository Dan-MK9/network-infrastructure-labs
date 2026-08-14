# 📍 Lab 01: Comunicação Básica em Rede Local

## 1. Descrição e Objetivo
O objetivo deste laboratório é validar os conceitos fundamentais de conectividade local (LAN), configurando o endereçamento IPv4 estático entre dois hosts interconectados por um switch de Camada 2 (Layer 2) e testando a comunicação via protocolo ICMP.

---

## 2. Diagrama de Topologia
<img width="909" height="440" alt="image" src="https://github.com/user-attachments/assets/35124098-a053-4673-83b5-1fc191f6e2f9" />


---

## 3. Tabela de Endereçamento
| Dispositivo | Interface | Endereço IP | Máscara de Sub-rede | Gateway Padrão |
| :--- | :--- | :--- | :--- | :--- |
| **PC0** | FastEthernet0 | 192.168.1.10 | 255.255.255.0 (/24) | N/A |
| **PC1** | FastEthernet0 | 192.168.1.20 | 255.255.255.0 (/24) | N/A |
| **Switch0** | Catalyst 2960 | Não gerenciado (L2 básico) | N/A | N/A |

---

## 4. Metodologia e Passos Realizados
1. **Montagem do Ambiente:** Conexão do `PC0` e `PC1` às portas FastEthernet do switch `2960-24TT`.
2. **Configuração dos Hosts:** Definição manual dos endereços IPv4 em ambos os computadores dentro da sub-rede `192.168.1.0/24`.
3. **Teste de Conectividade:** Execução do utilitário `ping` a partir do `PC0` com destino ao endereço `192.168.1.20`.

---

## 5. Validação e Resultados
* **Teste de Ping:**
  ```text
  Pinging 192.168.1.20 with 32 bytes of data:
  Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
  Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
  
  Ping statistics for 192.168.1.20:
      Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)

## 6. Análise Técnica
* **Por que os computadores conseguem se comunicar sem roteador?**
  > Ambos os dispositivos pertencem ao mesmo domínio de broadcast e à mesma sub-rede lógica (`192.168.1.0/24`). O switch opera na Camada 2 (Enlace), encaminhando quadros Ethernet baseado exclusivamente nos endereços MAC locais.
* **O que aconteceria se um dos hosts estivesse na faixa `192.168.2.x`?**
  > A comunicação direta não ocorreria, pois estariam em redes lógicas (Layer 3) distintas, necessitando de um roteador (Gateway Padrão) para o encaminhamento dos pacotes.
