
# Entrega Aula 1 — Grupo 10

**Disciplina:** Cloud & Cognitive Environments — FIAP MBA AI Engineering & Multi-Agents
**Turma:** <código da sua turma> # sla man
**Data de entrega:** <16/08/2026>

## Grupo

| # | Nome completo | GitHub | E-mail FIAP |
|---|---------------|--------|-------------|
| 1 | Natanael F. Ramos Filho | https://github.com/Natan5533| rm373022@fiap.com.br|
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

## Distribuição do trabalho

| Membro | Nível assumido | Item específico |
|--------|----------------|-----------------|
| Nome 1 | 🟢 N1 | Exercícios 1.1, 1.2, 1.3 |
| Nome 2 | 🟡 N2 | Exercício 2.1 — Arquitetura QC |
| Nome 3 | 🟡 N2 | Exercício 2.2 — Comparativo |
| Nome 4 | 🔴 N3 (bônus) | Exercício 3.1 — IaC avançado |
| Nome 5 | 🟢 N1 (apoio) | Revisão das respostas N1 |

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
| Google Kubernetes Engine (GKE) | PaaS/IaaS (híbrido) | Você gerencia os pods; GCP gerencia o plano de controle do K8s |
| Azure Blob Storage | PaaS | Concorrente direto do S3; O usuario gerencia os dados |
| Azure OpenAI Service | SaaS |  |

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
| Agente de IA que LÊ produtos do Storage para responder ao cliente | READER| |
| Engenheiro de dados que CARREGA novos catálogos no Blob | Contributor| |
| Time de FinOps que precisa VER custos sem alterar recursos | Reader | |
| Auditor externo que precisa LER configurações de toda a assinatura | Reader| |
| Sistema de CI/CD que provisiona infraestrutura via Terraform | Contributor/Owner | |


---

## 🟡 Nível 2 — Respostas + Implementação

(Respostas + diagramas + código quando aplicável)

---

## 🔴 Nível 3 — Bônus (se aplicável)

(Respostas + scripts/links)

---

## Reflexão coletiva

3-5 parágrafos respondendo:

1. O que o grupo aprendeu de mais importante nesta aula?
2. Como isso se conecta com a arquitetura cloud de uma plataforma agentic?
3. Que decisão arquitetural vocês fariam diferente se começassem o projeto QC hoje?

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