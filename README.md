# 🦂 Projeto Scorpion: Orquestração de Microserviços com AWS EKS & Kubernetes

Este repositório detalha a implementação de uma infraestrutura escalável utilizando o **Amazon Elastic Kubernetes Service (EKS)**. O foco do projeto foi garantir alta disponibilidade, resiliência e automação de deploy em larga escala.

---

## 🚀 Contexto do Projeto & Arquitetura

### 🔴 O Desafio
Hospedar uma aplicação crítica que exige **auto-healing** e escalabilidade horizontal. Uma solução baseada em instâncias simples não seria capaz de gerenciar o ciclo de vida dos containers de forma eficiente.

### 🟢 A Solução
Implementação de um cluster gerenciado **AWS EKS**. A arquitetura utiliza **Nodes** distribuídos em múltiplas zonas de disponibilidade (Multi-AZ), garantindo resiliência total e disponibilidade global para a aplicação Scorpion.

---

## 🛠️ Stack Tecnológica & Engenharia de Containers

| Ferramenta | Ícone | Justificativa Técnica |
| :--- | :---: | :--- |
| **Kubernetes** | <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/kubernetes/kubernetes-plain.svg" width="45"> | Orquestração de containers e gerenciamento de ReplicaSets. |
| **AWS EKS** | <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" width="45"> | Plano de controle gerenciado de alta disponibilidade. |
| **Docker** | <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original.svg" width="45"> | Padronização da aplicação em imagens imutáveis. |

---

## 📸 Evidências de Implementação (Case Study)

### 🔹 Infraestrutura como Serviço (EKS Cluster)
*Provisionamento do cluster e validação dos Worker Nodes.*
![EKS Setup](img/11.png)

---

### 🔹 Orquestração e Deploy de Workloads
*Gerenciamento de pods e deploy da aplicação via kubectl.*
![Kubectl Deploy](img/12.png)

---

### 🔹 Escalabilidade e Saúde do Cluster
*Monitoramento da distribuição de carga entre os nós do cluster.*
![EKS Nodes](img/13.png)

---

### 🔹 Aplicação Scorpion em Produção
*O resultado final: aplicação orquestrada e acessível globalmente.*
![Scorpion Live](img/14.png)

---

## 🏁 Conclusão
O Projeto Scorpion demonstra a maturidade na gestão de ambientes conteinerizados na nuvem AWS, focando em robustez e escalabilidade.

---
*Documentação desenvolvida por Gustavo Gomes | Cloud & DevOps Engineer*
