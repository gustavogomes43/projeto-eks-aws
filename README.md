# ☁️ AWS Cloud & Infrastructure Portfolio - Gustavo Gomes

Repositório focado em **Infraestrutura como Código (IaC)** e **Sustentação de Ambientes Críticos** na AWS.

---

## 🦂 Projeto Scorpion (Mission Critical EKS Cluster)

Este projeto demonstra o provisionamento completo de um cluster Kubernetes (EKS) utilizando Terraform, com foco em alta disponibilidade e segurança.

### 🛠️ Tecnologias e Ferramentas
| Ícone | Ferramenta | Descrição |
| :---: | :---: | :--- |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" width="35"> | **AWS EKS** | Orquestração de containers gerenciada. |
| <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/terraform/terraform-original.svg" width="35"> | **Terraform** | IaC para provisionamento de VPC, IAM e Clusters. |

### 📸 Galeria de Implementação (Passo a Passo)

#### 🔹 Fase 1: Infraestrutura e VPC
Nesta etapa, a VPC e as subnets foram desenhadas para suportar o tráfego do cluster.
* **Planejamento da Rede:** ![VPC Plan](img/1.png)
* **Provisionamento de Recursos:** ![Terraform Apply](img/2.png)
* **Mapa de Recursos da VPC:** ![Resource Map](img/14.png)

#### 🔹 Fase 2: Provisionamento do Cluster EKS
Execução do Terraform para criação do Control Plane e instâncias Worker.
* **Criação do Cluster em Progresso:** ![EKS Creating](img/3.png)
* **Finalização do Provisionamento (8m24s):** ![EKS Done](img/4.png)

#### 🔹 Fase 3: Gerenciamento de Nodes e Instâncias
Validação dos grupos de nós e tipos de instâncias utilizadas (t3.micro).
* **Node Groups Ativos:** ![Node Groups](img/12.png)
* **Detalhes da Instância EC2:** ![EC2 Details](img/13.png)

*(Nota: Adicione as outras imagens conforme sua preferência de organização seguindo o padrão acima).*

---

## 🎵 Projeto Aria.net (S3 Static Hosting)
Hospedagem serverless de alta performance.

---
*Este portfólio demonstra resiliência técnica e foco na sustentação de ambientes críticos.*
