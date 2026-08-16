# 📍 Lab 07: Rede Corporativa Integrada (VLANs, Roteamento, DHCP e DNS) 

## 1. Descrição e Objetivo
O objetivo deste laboratório é projetar e implementar a infraestrutura de rede completa de uma empresa simulada dividida em três departamentos (RH, Financeiro e TI), integrando conceitos avançados de segmentação em Camada 2 (VLANs), roteamento inter-VLAN em Camada 3 e centralização de serviços essenciais de rede (DHCP e DNS) em um servidor dedicado.

---

## 2. Topologia
<img width="1507" height="538" alt="topology" src="https://github.com/user-attachments/assets/8ce2fa7f-137a-4714-9d38-47720a28988c" />

---

## 3. Matriz de Segmentação e Endereçamento

| Departamento | VLAN ID | Rede IPv4 | Máscara | Gateway Padrão (Router) | Escala de Hosts |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Recursos Humanos (RH)** | 10 | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1` | 10 computadores |
| **Financeiro** | 20 | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1` | 10 computadores |
| **Tecnologia da Informação (TI)** | 30 | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1` | 10 computadores |
| **Serviços / Infraestrutura** | Nativa/MGMT | Conforme alocação | `255.255.255.0` | Roteador Central | Servidor DHCP/DNS |

---

## 4. Serviços de Infraestrutura (Servidor Central)

* **Serviço DHCP:** Pools configurados para distribuir dinamicamente parâmetros completos de rede (IP, Máscara, Default Gateway e apontamento do Servidor DNS) para os 30 computadores da empresa.
* **Serviço DNS:** Resolução de nomes internos (mapeamento de nomes FQDN para IPs), permitindo acesso a recursos sem memorização de endereços numéricos.

---

## 5. Metodologia e Etapas de Implementação
1. **Montagem da Topologia:** Distribuição de 1 roteador central, 4 switches para a infraestrutura de distribuição/acesso, 1 switch de acesso para o servidor e 30 hosts clientes.
2. **Segmentação L2:** Criação das VLANs 10 (RH), 20 (Financeiro) e 30 (TI) e atribuição das portas de acesso dos switches departamentais.
3. **Roteamento L3 (Inter-VLAN):** Configuração das interfaces/gateways no roteador central para viabilizar a comunicação controlada entre diferentes segmentos de rede.
4. **Provisionamento de Serviços:** Configuração e ativação dos escopos DHCP e zonas DNS no servidor.
5. **Validação:** Concessão bem-sucedida de endereçamento dinâmico nos hosts e testes de conectividade ICMP interdepartamentais.

---

## 6. Desafios Enfrentados e Aprendizados Técnicos
* **Compreensão do Fluxo Integrado:** Um dos principais desafios do cenário foi assimilar a cadeia completa de tráfego:
  $$\text{Host (Cliente)} \longrightarrow \text{VLAN (L2)} \longrightarrow \text{Gateway (L3)} \longrightarrow \text{Roteamento} \longrightarrow \text{Servidor DHCP/DNS}$$
* **Entrega de Serviços Multi-Rede:** Identificou-se na prática que a simples criação de VLANs isola os domínios de broadcast, exigindo que o tráfego de requisições de rede atravesse a camada 3 para alcançar servidores centrais e receber concessões de rede.

---

## 7. Análise Técnica
* **Qual é o papel do roteador em uma rede corporativa com múltiplas VLANs?**
  > As VLANs restringem o tráfego estritamente ao seu domínio de broadcast local (Camada 2). O roteador atua na Camada 3 realizando o encaminhamento dos pacotes entre as sub-redes distintas, permitindo que setores diferentes troquem dados de forma gerenciada e alcancem servidores centralizados de aplicação e infraestrutura.**
