
# Entrega Aula 1 — Grupo 10

**Disciplina:** Cloud & Cognitive Environments — FIAP MBA AI Engineering & Multi-Agents
**Turma:** 1AIE # 
**Data de entrega:** <16/08/2026>

## Grupo

| # | Nome completo | GitHub | E-mail FIAP |
|---|---------------|--------|-------------|
| 1 | Natanael F. Ramos Filho | https://github.com/Natan5533| rm373022@fiap.com.br|
| 2 | Gabriel A. Reis da Silva | https://github.com/gabriel-reis-silva| rm373082@fiap.com.br|
| 3 | Douglas Gomes Batista de Almeida | | rm371081@fiap.com.br|

## Distribuição do trabalho

| Membro | Nível assumido | Item específico |
|--------|----------------|-----------------|
| Natanael | 🟢 N1 | Exercícios 1.1, 1.2, 1.3 |
| Gabriel | 🟡 N2 | Exercício 2.1 — Arquitetura QC |
| Gabriel/Douglas | 🟡 N2 | Exercício 2.2 — Comparativo |
| N/A | 🔴 N3 (bônus) | Exercício 3.1 — IaC avançado |
| Douglas | 🟢 N1 (apoio) | Revisão das respostas N1 |

> Regra: cada membro deve ter pelo menos uma contribuição. O **rodízio entre aulas** (quem fez N1 antes faz N2 depois) é incentivado e vale o ponto do Critério 4 (ver [rubrica.md](rubrica.md)).

---

## 🟢 Nível 1 — Respostas

### *Exercício 1.1*

| Serviço | Modelo (IaaS/PaaS/SaaS/FaaS) | Justificativa |
|---------|------------------------------|---------------|
| Gmail | SaaS | Você apenas utiliza o serviço, não ha operação de infra|
| Azure Virtual Machines | IaaS | Azure prove o hardware e virtualização; Os usuarios cuidam da SO e outros componentes. |
| Azure App Service (hospedar uma API) | PaaS | o usuario é responsavel pela implementação do codigo com a regra de negocio. |
| AWS Lambda | FaaS | o usuario implementa uma função serveless. |
| Azure SQL Database | PaaS | O usuario fica responsavel pela gerencia das tabelas, schemas e indexes; Azure forneçe a instancia do banco |
| Salesforce CRM | SaaS | Usuario utiliza a ferramenta sem gerenciar a infra e codigo. |
| Google Kubernetes Engine (GKE) | PaaS/IaaS | Você gerencia as maquinas virtuais; GCP gerencia o plano de controle do Kubernetes |
| Azure Blob Storage | PaaS | Concorrente direto do S3; O usuario gerencia os dados |
| Azure OpenAI Service | SaaS | O usuario utiliza modelos de AI utilizando API  |

---

### Exercício 1.2 — Os 6 Rs na prática

**Cenário A:** Empresa de logística tem sistema de rastreamento de frotas em servidor físico próprio. Código de 2008, sem documentação, só uma pessoa sabe mexer. Quer migrar rápido para ganhar elasticidade.

REHOST - Ganho rapido porém sem os beneficios do REFACTOR; extrai pouco valor da cloud.


**Cenário B:** Banco regional usa ERP local de RH. Análise mostra: menos de 5 usuários ativos por mês, dados raramente consultados.
RETIRE - Não há necessidade de manter o sistema com essa quantidade de usuarios

**Cenário C:** Fintech tem API de pagamentos monolítica. Decide aproveitar a migração para refatorar em microserviços com K8s e event-driven.

REFACTOR - ganho elevado ja que o cloud fornece diversas ferramentas PaaS que podem ajudar no refatoramento(e.g SNS/SQS).

**Cenário D:** Varejo usa CRM desenvolvido internamente há 15 anos. SaaS de mercado atenderia 90% das necessidades por menor custo.

REPURCHASE - Sistemas internos tem um alto custo, sem o devida manutenção e investimento se tornam obsoletas, as opções do mercado serão mais completas e baratas no final do dia.

**Cenário E:** Instituição financeira tem mainframe com dados de clientes que precisa ficar on-premise por exigência do Banco Central.
RETAIN - Não há necessidade e demanda na migração e tambem envolve regras de compliance. 

### Exercício 1.3 — Calculando o impacto do SLA

Sistema de e-commerce com SLA de 99,9%.

a) Quantas horas de downtime por ano?
8 Horas 45 Minutos 36 Segundos

b) Se processa R$ 50.000/hora em vendas, qual o impacto financeiro máximo por ano?
formula: (50000 * 8) + ((50000/60) * 45) + (((50000/60)/ 60) * 36)

RR R$438.000


c) Para reduzir o impacto para menos de R$ 50.000/ano, qual SLA mínimo seria necessário?
99,99%


### Exercício 1.4 — RBAC na prática

Você é o responsável de segurança da Quantum Commerce. Para cada perfil abaixo, escolha a role built-in do Azure mais adequada e justifique:

| Perfil | Role Azure mais adequada | Justificativa |
|--------|--------------------------|---------------|
| Agente de IA que LÊ produtos do Storage para responder ao cliente | READER| Esse produto não precisa de permissões de escrita |
| Engenheiro de dados que CARREGA novos catálogos no Blob | Contributor| Acões de Write/Read são necessarias |
| Time de FinOps que precisa VER custos sem alterar recursos | Reader | Esses usuarios não precisam de permissão de escrita |
| Auditor externo que precisa LER configurações de toda a assinatura | Reader| Apenas necessita ler informações |
| Sistema de CI/CD que provisiona infraestrutura via Terraform | Contributor/Owner | A role de Contributor é a mais usada; Depedendo da execução o role de Owner se faz necessaria para gerenciar/criar  certos usuarios |


---

## 🟡 Nível 2 — Respostas + Implementação

### Exercício 2.1 — Arquitetura de alto nível: Quantum Commerce

**Contexto:** A Quantum Commerce é um gigante do e-commerce com 12 países, 5M de SKUs, e quer transformar a experiência de compra com IA conversacional.

**Sua tarefa (em grupo):** Proponha uma arquitetura de alto nível em cloud para a QC. Identifique:

1. **Camadas da arquitetura**
---
Para a arquitetura da Quantum Commerce, propomos **6 camadas principais**:

**Experiência / Frontend** — site e aplicativo que permitem pesquisa de produtos, compras e interação com a IA conversacional.

**API / Integração** — Azure API Management responsável por controlar, proteger e gerenciar o acesso às APIs da plataforma.

**Aplicação / Backend** — microsserviços Java + Spring Boot executados no AKS, responsáveis por funcionalidades como catálogo, clientes, pedidos, pagamentos e estoque.

**Dados** — bancos de dados, cache e Data Lake responsáveis pela persistência e disponibilização dos diferentes tipos de dados da plataforma.

**AI/ML** — Azure OpenAI integrado com RAG e serviços de orquestração para oferecer uma experiência de compra conversacional utilizando dados atualizados da Quantum Commerce, como catálogo, preços e estoque.

**Observabilidade** — Datadog ou Azure Monitor + Application Insights + Log Analytics para monitoramento de logs, métricas, traces, erros e performance dos serviços.

---
2. **Provedor principal** — qual escolheria
---
Escolhemos a Azure como provedor de cloud para atender a Quantum Commerce. Além de estar entre os principais provedores de cloud mundiais, a Azure possui forte integração com o ecossistema Microsoft, amplamente utilizado no mercado enterprise.
Além disso, os serviços propostos em nossa arquitetura, como AKS, API Management, Azure OpenAI e Azure Monitor, podem ser integrados dentro do mesmo ecossistema, facilitando a gestão e a evolução da solução.

---
3. **Serviços por categoria** — preencha a tabela:

| Categoria | Serviço Azure | Alternativa AWS | Alternativa GCP |
|-----------|--------------|-----------------|-----------------|
| Compute (backend) | Virtual Machines | EC2 | Compute Engine |
| Storage (catálogo, imagens) | Blob Storage | S3 | Cloud Storage |
| Banco relacional | Azure SQL | RDS/Aurora | Cloud SQL|
| Banco NoSQL | Cosmos DB | DynamoDB | Firestore |
| Vector Database | Azure AI Search| Amazon OpenSearch Service | Vertex AI Vector Search |
| Serviços de IA cognitivos | Azure AI Services | Bedrock + SageMaker | Vertex AI |
| CDN | Azure Front Door | Amazon Cloud Front | Cloud CDN |
| Mensageria/Filas | Azure Service Bus | Amazon MQ | Google Cloud Pub/Sub |
| Observabilidade (logs/métricas) | Azure Monitor | Amazon Cloud Watch | Google Cloud Monitoring |

4. **Diagrama**
---

<img width="899" height="602" alt="diagrama qc - cloud drawio" src="https://github.com/user-attachments/assets/91cbb54d-6d8c-4ff7-8319-09c03053664e" />


---

### Exercício 2.2 — Comparativo de custos: 3 provedores

Você precisa recomendar infraestrutura para um projeto de AI Engineering. Use as calculadoras para comparar:

* 2 VMs com 2 vCPUs e 8 GB RAM (Linux, 24/7)
* 500 GB de object storage
* 1 banco gerenciado com 2 vCPUs / 8 GB RAM / 100 GB
* 10 milhões de requisições/mês para função serverless

| Item               | Azure            | AWS              | GCP              | Notas                                              |
| ------------------ | ---------------- | ---------------- | ---------------- | -------------------------------------------------- |
| 2 × VM (2vCPU/8GB) | US$ 121,48       | US$ 121,48       | US$ 97,84        | Tipo: B2ms / t3.large / e2-standard-2              |
| 500 GB storage     | US$ 9,20         | US$ 11,50        | US$ 10,00        | Tipo: Blob Storage / S3 / Cloud Storage            |
| Banco gerenciado   | US$ 110,50       | US$ 131,00       | US$ 118,00       | Tipo: PostgreSQL Flexible Server / RDS / Cloud SQL |
| 10M req serverless | US$ 1,80         | US$ 1,80         | US$ 3,20         | Tipo: Functions / Lambda / Cloud Run Functions     |
| **Total mensal**   | **US$ 242,98**   | **US$ 265,78**   | **US$ 229,04**   |                                                    |
| **Total anual**    | **US$ 2.915,76** | **US$ 3.189,36** | **US$ 2.748,48** |                                                    |

**Análise:**

**a) Qual provedor ficou mais barato? A diferença é significativa?**

O **GCP ficou mais barato**, com aproximadamente US$ 229/mês. A diferença para Azure não é significativa, mais ou menos US$ 14/mês. Para AWS, a diferença é de aproximadamente US$ 37/mês.

**b) Aplicando Reserved Instances de 1 ano no mais caro, o resultado muda?**

Sim. A AWS foi a mais cara no modelo On-Demand. Com Reserved Instances de 1 ano, o custo pode cair para aproximadamente **US$ 180–190/mês**, podendo se tornar a opção mais barata.

**c) Além de preço, que outros fatores você consideraria para um projeto de IA?**

* Disponibilidade e custo de GPUs.
* Serviços de IA oferecidos por cada cloud.
* Escalabilidade.
* Segurança e compliance.
* Integração com outros serviços.
* Região e latência.
* Conhecimento da equipe sobre o provedor.

**Calculadoras:**

* Azure: https://azure.microsoft.com/pricing/calculator
* AWS: https://calculator.aws
* GCP: https://cloud.google.com/products/calculator

---


### Exercício 2.3 — Estratégia de migração para sua empresa

Pense no seu contexto profissional atual (ou empresa que conhece bem).

a) Descreva um sistema/workload:
---

O workload escolhido consiste em um conjunto de APIs e microsserviços desenvolvidos principalmente em Java + Spring Boot e executados em containers através de Kubernetes. O ambiente utilizava Rancher para gerenciamento dos clusters e das aplicações, além de Helm Charts para configuração dos deployments e pipelines de CI/CD para realização dos deploys.
Foi realizada uma migração desse ambiente para a Azure, passando a utilizar o Azure Kubernetes Service (AKS) como plataforma gerenciada para execução dos workloads.

---
b) Qual dos 6 Rs você aplicaria? Justifique custo, risco, ganho, prazo
---
A estratégia utilizada pode ser classificada como Replatform, pois as aplicações continuaram utilizando containers e Kubernetes, sem necessidade de uma grande reescrita do código, mas a plataforma utilizada para execução dos workloads foi alterada para o AKS.

Em relação ao custo, a utilização de um serviço gerenciado como o AKS reduz parte do esforço operacional necessário para gerenciamento da infraestrutura Kubernetes, embora introduza os custos dos recursos consumidos na Azure.

O risco é menor quando comparado a uma estratégia de Refactor, pois não é necessário reescrever completamente as aplicações. Entretanto, ainda existem riscos relacionados à configuração do novo cluster, networking, permissões, Helm Charts e pipelines de deploy.

Como ganho, temos maior integração com o ecossistema Azure, além dos benefícios de utilizar Kubernetes como serviço gerenciado.

Em relação ao prazo, Replatform tende a exigir mais trabalho que um simples Rehost, devido às adaptações necessárias, mas é significativamente menos complexo do que uma refatoração completa da aplicação.

---
c) Que serviço Azure usaria? Estimativa mensal?
---
Utilizaria o Azure Kubernetes Service (AKS) para hospedar os workloads em Kubernetes. Para representar um ambiente produtivo com maior disponibilidade, considerei 3 nodes, com execução 24/7.

Na calculadora da Azure, essa configuração resultou em um custo mensal estimado de US$ 607,36, totalizando aproximadamente US$ 7.288,32 por ano. O valor considera a infraestrutura principal do cluster e pode aumentar conforme o uso de armazenamento, tráfego de rede, monitoramento e outros serviços.

---
d) Maior obstáculo técnico ou organizacional? Como endereçaria?
---
O principal obstáculo técnico é garantir que os workloads que funcionavam no ambiente Kubernetes anterior continuem funcionando corretamente no AKS. Apesar de ambos utilizarem Kubernetes, existem diferenças de infraestrutura, rede, configurações, permissões e integrações.

Para reduzir esse risco, a migração deve ser realizada gradualmente, revisando os Helm Charts e pipelines de CI/CD, validando os workloads em ambientes não produtivos e realizando testes antes da migração do ambiente produtivo.

---

## 🔴 Nível 3 — Bônus (se aplicável)

(Respostas + scripts/links)

---

## Reflexão coletiva

1. O que o grupo aprendeu de mais importante nesta aula?

O que aprendemos de mais importante na aula, foi que temos diversas possibilidades em todos os provedores de nuvem. 
E justamente por essa diversidade, devemos ter o escopo do projeto bem definido e analisar não só as possibilidades mas também os custos. Além disso, é importante ressaltar que o uso dos 6 R's deve ser bem entendido e assim podemos definir o que fazer e se faz sentido ou não. 

2. Como isso se conecta com a arquitetura cloud de uma plataforma agentic?

Isso se conecta com uma plataforma agentic porque precisamos entender suas necessidades antes de escolher os serviços de cloud. Como agentes podem depender de processamento, dados, APIs e serviços de IA, conhecer as opções e custos de cada provedor ajuda a criar uma arquitetura eficiente, escalável e sem custos demais.

4. Que decisão arquitetural vocês fariam diferente se começassem o projeto QC hoje?

Pensamos em uma arquitetura voltada a microsserviços. Por ser uma plataforma presente em diversos países e com um grande volume de produtos e usuários, poderíamos escalar horizontalmente os serviços de acordo com a demanda. 

---

## Artefatos do ZIP

- Diagrama: `diagramas/arquitetura-qc-aulaXX.png`
- Código IaC: `terraform/`
- Scripts: `scripts/`
- Endpoint ativo (se houver): URL pública sem credenciais — apenas para demonstração durante a janela de correção
```

---

## Lembretes ao gerar o ZIP

- Nome do ZIP: `entrega-grupo-NN-aulaXX.zip` (substitua NN e XX)
- Estrutura interna: pasta única `qc-grupo-NN-aulaXX/` no topo
- Tamanho ideal: < 5 MB
- **NÃO incluir:** `terraform.tfstate*`, `.env`, `*.pem`, `__pycache__/`, `.venv/`

Comando recomendado no Cloud Shell:

```bash
cd ~/qc-grupo-NN
git pull origin main
git archive --format=zip --prefix=qc-grupo-NN-aulaXX/ -o ~/entrega-grupo-NN-aulaXX.zip HEAD:aulaXX
unzip -l ~/entrega-grupo-NN-aulaXX.zip   # conferir o que entrou
```

Upload do ZIP no Portal FIAP (apenas 1 membro do grupo faz).
