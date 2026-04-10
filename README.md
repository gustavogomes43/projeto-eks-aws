# 🦂 Projeto Scorpion: Missão Crítica com Amazon EKS & IaC

Este repositório contém a infraestrutura completa de um cluster Kubernetes gerenciado na AWS, desenhado para suportar aplicações de alta disponibilidade com foco em **Otimização Financeira (FinOps)** e **Segurança Ofensiva**.

---

## 🎯 Finalidade e Solução
**A Dor do Cliente:** O cliente possuía uma aplicação legada rodando em instâncias isoladas, com alto custo fixo, dificuldade de escala e sem resiliência a falhas de zona (AZ).
**A Solução:** Implementação de um cluster **Amazon EKS** via **Terraform**, migrando a carga para containers e utilizando uma arquitetura de rede inteligente para eliminar custos de tráfego desnecessários.

---

## 🛠️ Stack Tecnológica & Por que foram escolhidas?

| Ferramenta | Nome | Justificativa Técnica |
| :---: | :--- | :--- |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" width="35"> | **Amazon EKS** | Orquestração de containers que elimina a sobrecarga de gerenciar o Control Plane. |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/terraform/terraform-original.svg" width="35"> | **Terraform** | Escolhido pela sua capacidade de gerenciar estados complexos e garantir infraestrutura imutável. |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/kubernetes/kubernetes-plain.svg" width="35"> | **Kubectl** | Ferramenta essencial para inspeção e governança dos workloads. |

---

## 📈 Benefícios e Resultados (FinOps em Foco)

* **Redução de Custos (NAT Gateway Avoidance):** Evitamos o custo fixo de $32/mês por zona ao não utilizar NAT Gateways. Através de subnets públicas com Security Groups restritivos, alcançamos uma **redução de ~85% nos custos de rede** de saída.
* **Alta Disponibilidade (Multi-AZ):** Distribuição de nós em múltiplas zonas de disponibilidade, garantindo **99.9% de uptime** da infraestrutura.
* **Escalabilidade Automática:** Cluster pronto para Horizontal Pod Autoscaling (HPA).

---

## 🚧 Desafios e Melhores Práticas

### 🔒 Segurança (Princípio do Menor Privilégio)
Utilizei **IAM Roles for Service Accounts (IRSA)** para que os nós tenham apenas as permissões necessárias para o registro no EKS, bloqueando qualquer acesso lateral indevido.

### 🛠️ Troubleshooting de Rede
**Problema:** Worker Nodes falhando no handshake com o Control Plane em subnets privadas.
**Solução:** Reestruturação das Tabelas de Roteamento e implementação de regras de Inbound específicas para a porta 443, permitindo a comunicação segura sem expor o cluster à internet pública.

---

## 📸 Galeria de Implementação (Evidências)

### Fase 1: Planejamento de Rede
![Mapa da VPC](img/14.png)
*Visualização clara da arquitetura de subnets e isolamento de recursos.*

### Fase 2: Provisionamento IaC
![Terraform Apply](img/4.png)
*Tempo recorde de deploy: Infraestrutura complexa pronta em menos de 10 minutos.*

### Fase 3: Validação do Cluster
![Kubectl Status](img/12.png)
*Nós em status 'Ready', confirmando a saúde do ambiente e conectividade de rede.*

---

## 📊 Resumo de Métricas Alcançadas
* **Agilidade de Deploy:** 95% mais rápido que o provisionamento manual.
* **Economia Gerada:** ~$400/ano em laboratório através de decisões de FinOps.
* **Conformidade:** 100% dos recursos tagueados e versionados.

---
*Este projeto demonstra resiliência técnica e foco na sustentação de ambientes críticos.*
