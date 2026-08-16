# 📍 Lab 05: Segmentação de Rede Local com VLANs

## 1. Descrição e Objetivo
O objetivo deste laboratório é implementar a segmentação lógica de rede na Camada 2 (Layer 2) por meio de VLANs (Virtual Local Area Networks), isolando o tráfego departamental entre os setores de Recursos Humanos (RH) e Tecnologia da Informação (TI) em um mesmo switch físico.

---

## 2. Diagrama de Topologia
<img width="750" height="312" alt="topology" src="https://github.com/user-attachments/assets/652dce57-be64-488f-aa8d-b1ce020f7a3f" />


---

## 3. Tabela de Endereçamento e Alocação de VLANs
| Departamento | Dispositivo | Interface Switch | VLAN ID | Nome da VLAN | Endereço IP | Máscara de Sub-rede |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **RH** | PC0 | FastEthernet0/1 | 10 | VLAN10 (RH) | 192.168.1.10 | 255.255.255.0 (/24) |
| **RH** | PC1 | FastEthernet0/2 | 10 | VLAN10 (RH) | 192.168.1.20 | 255.255.255.0 (/24) |
| **TI** | PC2 | FastEthernet0/3 | 20 | VLAN20 (TI) | 192.168.1.10 | 255.255.255.0 (/24) |
| **TI** | PC3 | FastEthernet0/4 | 20 | VLAN20 (TI) | 192.168.1.20 | 255.255.255.0 (/24) |

---

## 4. Configuração das VLANs e Portas de Acesso (Switch)

    ! Criação das VLANs no Banco de Dados / Modo de Configuração
    Switch# configure terminal
    Switch(config)# vlan 10
    Switch(config-vlan)# name RH
    Switch(config-vlan)# exit
    Switch(config)# vlan 20
    Switch(config-vlan)# name TI
    Switch(config-vlan)# exit

    ! Atribuição das portas do departamento de RH à VLAN 10
    Switch(config)# interface range fastethernet 0/1 - 2
    Switch(config-if-range)# switchport mode access
    Switch(config-if-range)# switchport access vlan 10
    Switch(config-if-range)# exit

    ! Atribuição das portas do departamento de TI à VLAN 20
    Switch(config)# interface range fastethernet 0/3 - 4
    Switch(config-if-range)# switchport mode access
    Switch(config-if-range)# switchport access vlan 20
    Switch(config-if-range)# exit

---

## 5. Metodologia e Passos Realizados
1. **Conexões Físicas:** Conexão dos computadores do setor de RH nas interfaces `Fa0/1` e `Fa0/2`, e dos computadores do setor de TI nas interfaces `Fa0/3` e `Fa0/4` do switch `2960-24TT`.
2. **Criação de VLANs:** Configuração manual das VLANs 10 (RH) e 20 (TI) na base do switch.
3. **Mapeamento de Acesso:** Alteração das portas de acesso da VLAN 1 padrão para suas respectivas VLANs departamentais.
4. **Testes de Isolamento e Conectividade:** Execução de testes de ping entre hosts da mesma VLAN e validação de isolamento entre VLANs diferentes.

---

## 6. Validação e Resultados
* **Comunicação Intra-VLAN:** Disparo de ping a partir do `PC0` (VLAN 10) em direção ao `PC1` (VLAN 10) concluído com 0% de perda (`time<1ms`, `TTL=128`).
* **Isolamento Inter-VLAN:** Dispositivos em VLANs distintas permanecem isolados na Camada 2, garantindo contenção de broadcast e conformidade de segurança departamental.

---

## 7. Análise Técnica
* **Por que hosts em VLANs diferentes não se comunicam diretamente?**
  > As VLANs particionam a tabela de comutação do switch em múltiplos domínios de broadcast isolados na Camada 2. Mesmo compartilhando o mesmo hardware físico, o switch bloqueia o tráfego direto entre VLANs distintas, demandando um dispositivo de Camada 3 (como um roteador via sub-interfaces ou switch L3) para rotear os pacotes.
