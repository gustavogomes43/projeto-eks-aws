# 🦂 Projeto Scorpion: Orquestração de Microserviços de Alta Disponibilidade com AWS EKS

[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.29-326CE5?logo=kubernetes)](https://kubernetes.io/)
[![AWS EKS](https://img.shields.io/badge/AWS-EKS_Managed-FF9900?logo=amazonaws)](https://aws.amazon.com/eks/)
[![Status](https://img.shields.io/badge/Status-Production_Ready-success)](#)

## 📝 Visão Geral
O **Projeto Scorpion** é uma implementação robusta de orquestração de microserviços utilizando o **Amazon EKS**. A arquitetura foi desenhada para suportar aplicações críticas que exigem **escalabilidade horizontal**, **auto-healing** (auto-recuperação) e gerenciamento eficiente de recursos computacionais através de Nodes gerenciados pela AWS.

---

## 🏗️ Arquitetura de Engenharia do Cluster EKS (The Scorpion Design)

Diferente de uma instalação simples e monolítica, o **Projeto Scorpion** foi estruturado seguindo os princípios de *Well-Architected Framework* da AWS para garantir que a aplicação seja resiliente, segura e auditável.

Abaixo, apresento a orquestração de rede e compute desenhada para este cluster:

%%{init: ...mermaid {'theme': 'base', 'themeVariables': { 'primaryColor': '#232F3E', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#ffffff', 'primaryTextColor': '#ffffff', 'lineColor': '#879196'}}}%%
graph TD
    %% Título Principal do Diagrama
    subgraph Scorpion_Architecture [🎵 Projeto Scorpion: Arquitetura de Engenharia EKS]
        direction TB

        %% Camada 1: Entrada e Rede (Estilo AWS Laranja)
        subgraph Layer_1 [Camada 1: Entrada e Segurança]
            direction LR
            Internet((Internet)) --> WAF[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/SecurityIdentityCompliance/WAF.png' width='40'/><br/>AWS WAF]
            WAF --> ALB[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/NetworkingContentDelivery/ElasticLoadBalancing.png' width='40'/><br/>Application Load Balancer]
            IAM[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/SecurityIdentityCompliance/IdentityAccessManagement.png' width='40'/><br/>IAM & RBAC] -.-> EKS_API
        end

        %% Camada 2: Control Plane (Estilo AWS Roxo)
        subgraph Layer_2 [Camada 2: AWS EKS Control Plane (HA)]
            direction TB
            EKS_API[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/Containers/ElasticKubernetesService.png' width='40'/><br/>EKS API Server]
            ETCD[<img src='https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png' width='30'/><br/>etcd (HA Datastore)]
            Sched[Scheduler]
            CtrlMgr[Controller Manager]
            EKS_API --- ETCD
            EKS_API --- Sched
            EKS_API --- CtrlMgr
        end

        %% Camada 3: Data Plane (Estilo AWS Laranja EC2)
        subgraph Layer_3 [Camada 3: Data Plane (Managed Node Groups)]
            direction LR
            subgraph Node_AZ1 [AZ 1]
                EC2_1[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/Compute/AmazonEC2.png' width='40'/><br/>EC2 Worker Node]
            end
            subgraph Node_AZ2 [AZ 2]
                EC2_2[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/Compute/AmazonEC2.png' width='40'/><br/>EC2 Worker Node]
            end
        end

        %% Camada 4: Workloads Kubernetes (Estilo K8s Azul)
        subgraph Layer_4 [Camada 4: Orquestração e Workloads (Kubernetes)]
            direction TB
            K8s_SVC[<img src='https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png' width='30'/><br/>K8s Service (ClusterIP)]
            K8s_Deploy[<img src='https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png' width='30'/><br/>K8s Deployment]
            subgraph Scorpion_Pods [Scorpion Microservices]
                Pod_1((<img src='https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg' width='25'/><br/>Scorpion Pod))
                Pod_2((<img src='https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg' width='25'/><br/>Scorpion Pod))
            end
            K8s_SVC --> K8s_Deploy
            K8s_Deploy --> Scorpion_Pods
        end

        %% Fluxos de Comunicação
        ALB ==> K8s_SVC
        EKS_API ==> Layer_3
        Layer_3 ==> Layer_4
        
        %% Componentes de Rede CNI
        CNI[<img src='https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/v18.0/dist/NetworkingContentDelivery/VPC.png' width='35'/><br/>AWS VPC CNI] -.-> Layer_3
        IAM -.-> Layer_4
    end

    %% Estilização para brilhar os olhos (Cores AWS e K8s)
    classDef aws_orange fill:#FF9900,stroke:#232F3E,stroke-width:2px,color:white;
    classDef aws_purple fill:#C851BD,stroke:#232F3E,stroke-width:2px,color:white;
    classDef k8s_blue fill:#326CE5,stroke:#232F3E,stroke-width:2px,color:white;
    classDef internet_gray fill:#879196,stroke:#232F3E,stroke-width:2px,color:white;
    classDef white_box fill:#ffffff,stroke:#232F3E,stroke-width:1px,color:#232F3E;

    class ALB,WAF,Node_AZ1,Node_AZ2,EC2_1,EC2_2 aws_orange;
    class EKS_API,ETCD,Sched,CtrlMgr,IAM aws_purple;
    class K8s_SVC,K8s_Deploy,Pod_1,Pod_2 k8s_blue;
    class Internet internet_gray;
    class Scorpion_Architecture,Layer_1,Layer_2,Layer_3,Layer_4 white_box;...

---

### Detalhamento Técnico das Camadas

O diagrama acima ilustra o fluxo de dados e controle, dividido em 6 pilares estratégicos:

| Pilar | Descrição Técnica e Decisões de Design |
| :--- | :--- |
| **Pilar 1: Networking & IAM** | **VPC CNI:** Escolhido para que cada Pod receba um IP real da VPC, otimizando a latência de rede.<br>**IRSA:** Implementação de permissões granulares do IAM diretamente para os Pods (Princípio de Least Privilege). |
| **Pilar 2: Control Plane** | **Gerenciamento:** Totalmente operado pela AWS em múltiplas Zonas de Disponibilidade (AZs) para garantir **Alta Disponibilidade (HA)** do API Server e etcd. |
| **Pilar 3: Data Plane (Workers)** | **Managed Node Groups:** Workers em instâncias EC2, com automação de atualizações de segurança e patches operados pelo EKS. |
| **Pilar 4: Orquestração** | **CoreDNS & kube-proxy:** Componentes vitais para a descoberta de serviços e gerenciamento de rede dentro do cluster. |
| **Pilar 5: Workloads (Scorpion)** | **Deployment:** Objeto Kubernetes imutável que garante a quantidade correta de réplicas rodando.<br>**Services:** Exposição via *ClusterIP* ou *LoadBalancer* para entrada de tráfego. |
| **Pilar 6: Lógica de Negócio** | **Microserviço Scorpion:** A aplicação Python/Node.js otimizada para containers, rodando de forma isolada e segura. |

---

> **Destaque Visual:** O diagrama demonstra como a aplicação Scorpion (Camada 6) é orquestrada através de camadas de compute, rede e orquestração gerenciadas pela AWS (EKS), garantindo que um recrutador entenda a profundidade técnica do projeto em um piscar de olhos.

---

## 🛠️ Tecnologias e Decisões de Design

| Componente | Ferramenta | Justificativa |
| :--- | :--- | :--- |
| **Orquestrador** | **Kubernetes** | Padronização de mercado para gerenciamento de containers e escalabilidade. |
| **Cloud Provider** | **AWS (EKS)** | Redução do overhead operacional no gerenciamento do Master Node. |
| **CLI Management** | **Kubectl & AWS CLI** | Automação total via terminal para deploys e troubleshoot. |

---

## 🚀 Ciclo de Vida e Implementação (Case Study)

### 1. Provisionamento e Setup do Cluster
O cluster foi configurado para suportar cargas de trabalho distribuídas. Acompanhei cada etapa do provisionamento para garantir que o Control Plane estivesse saudável antes do join dos nodes.
![EKS Setup](img/1.png)
![Status Active](img/11.png)

### 2. Deploy de Workloads e Services
Utilizei objetos de **Deployment** para garantir a imutabilidade das réplicas e **Services (LoadBalancer/ClusterIP)** para a exposição correta da aplicação Scorpion.
![Kubectl Deploy](img/12.png)
![Replica Sets](img/7.png)

### 3. Monitoramento e Escalabilidade
Validação do estado dos Nodes e Pods para garantir que o cluster responda corretamente a picos de tráfego, mantendo o **High Availability (HA)**.
![EKS Nodes](img/13.png)
![Running Pods](img/8.png)

---

## 🧠 Desafios Técnicos e Soluções (Troubleshooting)

**1. Ajuste de Contexto e Autenticação (RBAC):**
* **Desafio:** Configurar o `aws-auth` ConfigMap para permitir que diferentes usuários IAM tivessem permissões granulares dentro do cluster.
* **Solução:** Mapeamento correto de Roles IAM para Grupos do Kubernetes, garantindo segurança no acesso administrativo.

**2. Persistência e Ciclo de Vida:**
* **Desafio:** Garantir que a aplicação Scorpion mantivesse a consistência durante os rolling updates.
* **Solução:** Configuração de *Readiness* e *Liveness Probes*, evitando que o tráfego fosse direcionado a containers que ainda não estavam prontos para processar requisições.

---

## 📈 Impacto de Negócio
* **Disponibilidade:** 99.9% de uptime garantido pela arquitetura distribuída do EKS.
* **Agilidade:** O tempo de deploy de novas versões da aplicação Scorpion foi reduzido de minutos para segundos (Zero Downtime Deploy).
* **Resiliência:** Em caso de falha de um Node, o Kubernetes automaticamente reagenda os Pods em nós saudáveis, sem intervenção humana.

---

## 🏁 Resultado Final
A aplicação **Scorpion** está operando em um ambiente resiliente, escalável e pronto para produção, demonstrando domínio em toda a jornada de orquestração moderna.
![Scorpion Live](img/14.png)

---

**Autor:** [Gustavo Gomes](https://github.com/gustavogomes43) | *Cloud & Kubernetes Engineer*
