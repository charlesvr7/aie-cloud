# Exercícios — Aula 1

**Tema:** Fundamentos de Cloud & Infraestrutura como Código
**Formato:** **Entrega obrigatória por grupo** — ZIP no Portal FIAP
**Vale:** 10% da nota final ([rubrica completa](../../entregas/rubrica.md))
**Prazo:** 1 dia antes da Aula 2
**Como entregar:** ver [entregas/entrega-01/INSTRUCOES.md](../../entregas/entrega-01/INSTRUCOES.md) e [pos-aula-git.md](pos-aula-git.md)

---

## Instruções gerais

Esta é a **primeira entrega de grupo** da disciplina. Ao todo são 5 entregas intermediárias (10% cada) + projeto integrado final entregue como ZIP **1 semana após a Aula 6** (50%, **sem apresentação oral**).

### Como o grupo se organiza

Os 3 níveis de exercícios são **divisão de trabalho dentro do grupo**, não escolha individual livre:

- 🟢 **Nível 1 — Básico:** consolidação dos conceitos da aula (modelos de serviço, 6 Rs, SLA, RBAC)
- 🟡 **Nível 2 — Intermediário:** bloco do projeto Quantum Commerce (arquitetura, comparativo de custos, migração)
- 🔴 **Nível 3 — Avançado:** **bônus opcional** — Network Security via Terraform, Bicep equivalente, multi-cloud

**Mínimo obrigatório:** N1 + N2 cobertos. **N3 é bônus** (até +2 pts extras na nota da aula).

### Distribuição sugerida (4-5 membros)

- Membros com menos experiência em cloud: assumem N1 (consolidação)
- Membros com experiência intermediária: assumem N2 (projeto QC)
- Membros experientes: assumem N3 (bônus) — se houver perfil avançado no grupo

> Quem fez N1 desta aula deve preferencialmente fazer N2 ou N3 na próxima — **rodízio entre aulas vale ponto** no Critério 4 da rubrica.

### Template obrigatório

Use o [template em `entregas/template-entrega-grupo.md`](../../entregas/template-entrega-grupo.md) — inclui cabeçalho de grupo, distribuição de trabalho, respostas dos níveis e reflexão coletiva.

> **Política "no install":** Todos os exercícios podem ser feitos no Azure Cloud Shell + editor `code` (ou github.dev). Não instale nada localmente.

---

## 🟢 Nível 1 — Básico: Consolidando os Fundamentos

### Exercício 1.1 — Mapeamento de modelos de serviço

Para cada serviço, identifique se é IaaS, PaaS, SaaS ou FaaS. Justifique em uma frase.

| Serviço | Modelo (IaaS/PaaS/SaaS/FaaS) | Justificativa |
|---------|------------------------------|---------------|
| Gmail | | |
| Azure Virtual Machines | | |
| Azure App Service (hospedar uma API) | | |
| AWS Lambda | | |
| Azure SQL Database | | |
| Salesforce CRM | | |
| Google Kubernetes Engine (GKE) | | |
| Azure Blob Storage | | |
| Azure OpenAI Service | | |

**Gabarito esperado:**

<details>
<summary>Clique para ver o gabarito</summary>

| Serviço | Modelo | Justificativa |
|---------|--------|---------------|
| Gmail | SaaS | Aplicação completa entregue ao usuário; você não gerencia nada da infra |
| Azure Virtual Machines | IaaS | Você gerencia SO, patches e aplicações; Azure só fornece hardware virtualizado |
| Azure App Service | PaaS | Azure gerencia SO e runtime; você só implanta o código |
| AWS Lambda | FaaS | Você escreve apenas a função; sem servidor fixo, paga por execução |
| Azure SQL Database | PaaS | Azure gerencia motor de banco; você gerencia dados e esquema |
| Salesforce CRM | SaaS | Aplicação completa entregue como serviço |
| GKE | PaaS/IaaS (híbrido) | Você gerencia os pods; GCP gerencia o plano de controle do K8s |
| Azure Blob Storage | PaaS (a maioria considera) | Storage gerenciado; você gerencia dados e políticas |
| Azure OpenAI Service | SaaS / API-as-a-Service | API pronta — você consome via REST sem gerenciar modelo ou infra |

</details>

---

### Exercício 1.2 — Os 6 Rs na prática

Leia cada cenário e escolha o R de migração mais adequado (Rehost, Replatform, Refactor, Repurchase, Retire, Retain). Justifique.

**Cenário A:** Empresa de logística tem sistema de rastreamento de frotas em servidor físico próprio. Código de 2008, sem documentação, só uma pessoa sabe mexer. Quer migrar rápido para ganhar elasticidade.

**Cenário B:** Banco regional usa ERP local de RH. Análise mostra: menos de 5 usuários ativos por mês, dados raramente consultados.

**Cenário C:** Fintech tem API de pagamentos monolítica. Decide aproveitar a migração para refatorar em microserviços com K8s e event-driven.

**Cenário D:** Varejo usa CRM desenvolvido internamente há 15 anos. SaaS de mercado atenderia 90% das necessidades por menor custo.

**Cenário E:** Instituição financeira tem mainframe com dados de clientes que precisa ficar on-premise por exigência do Banco Central.

---

### Exercício 1.3 — Calculando o impacto do SLA

Sistema de e-commerce com SLA de 99,9%.

a) Quantas horas de downtime por ano?  
  8,76 horas.
b) Se processa R$ 50.000/hora em vendas, qual o impacto financeiro máximo por ano?
   R$438000,00 ano
) Para reduzir o impacto para menos de R$ 50.000/ano, qual SLA mínimo seria necessário?
Sem dúvida a SLA 99,999%

> **Fórmulas úteis:**
> Downtime anual (horas) = 8.760 × (1 - SLA/100)
> Downtime mensal (min) = 43.800 × (1 - SLA/100)

---

### Exercício 1.4 — RBAC na prática

Você é o responsável de segurança da Quantum Commerce. Para cada perfil abaixo, escolha a role built-in do Azure mais adequada e justifique:

| Perfil -----------------------------------------------------------| Role Azure mais adequada | Justificativa |
|-------------------------------------------------------------------|--------------------------|---------------|
| Agente de IA que LÊ produtos do Storage para responder ao cliente | Reader                   | Necessário somente Leitura |
| Engenheiro de dados que CARREGA novos catálogos no Blob           | Contributor              | Mecessario leitura e escrita para salver arquivos no Blob|
| Time de FinOps que precisa VER custos sem alterar recursos        | Reader                   | Somente Visualização nao tem necessidade de escrita|
| Auditor externo que precisa LER configurações de toda a assinatura | Reader                  | Precisa somente Leitura para autidar|
| Sistema de CI/CD que provisiona infraestrutura via Terraform       |  Contributor            | Não tem necessidade de tudo, inclusive dar permissões aos outros ate dos HTL, utilizar permissão via Grupo de Recursos para melhor canalizar as permissões|

**Referência:** [Azure Built-in Roles](https://learn.microsoft.com/azure/role-based-access-control/built-in-roles)
---

## 🟡 Nível 2 — Intermediário: Análise e Estratégia

### Exercício 2.1 — Arquitetura de alto nível: Quantum Commerce

**Contexto:** A Quantum Commerce é um gigante do e-commerce com 12 países, 5M de SKUs, e quer transformar a experiência de compra com IA conversacional.

**Sua tarefa (em grupo):** Proponha uma arquitetura de alto nível em cloud para a QC. Identifique:

1. **Camadas da arquitetura** — quantas e o que cada uma faz (ex: frontend, API, dados, AI/ML, observabilidade)

Backend /API Gateway    -   Exposição de APIs, orquestração de serviços, regras de negócio, códigos.
FrontEnd   -   Sistema Web, mobile app e interfaces de atendimento "CRM".
Dados Transicionais - Pedidos, clientes, produtos, estoque e pagamento;
Dados Analiticos    - Data Lake, BI, Eventos , comportamento, Ciencia de dados. 
Rede CDN / Edge- Distribuição Global , redundancia, cache e baixa latência.
Observalidade  - logs, Monitoramento , relatorios , alertas, performance.
IA Conversacional - LLM, RAG, embeddings, recomendação e atendimento.
Security / Identidade -  VPN, Firewall , IAM, RBAC , Políticas de Acesso. 

2. **Provedor principal** 

O provedor principal escolhido foi a AWS. Porque é uma plataforma com ampla presença global, alta maturidade em serviços cloud, grande catálogo de soluções gerenciadas e forte capacidade de escalar workloads de e-commerce em múltiplas regiões. Para a Quantum Commerce, que atua em 12 países e possui 5 milhões de SKUs, a AWS permite combinar serviços de compute, storage, bancos de dados, CDN, mensageria, observabilidade e IA generativa com alta disponibilidade e controle de custo. 


3. **Serviços por categoria** — preencha a tabela:

| Categoria                       | Serviço Azure | Alternativa AWS | Alternativa GCP |
|---------------------------------|------------------------|-------------------------|-----------------|
| Compute (backend)               |Azure App Service       | Elastic Beanstalk       | Cloud Run       |
| Storage (catálogo, imagens)     | Azure Blob Storage     | Amazon S3               | Cloud Storage   |
| Banco relacional                | Azure SQL Database     | Amazon RDS / Aurora     | Cloud SQL       |
| Banco NoSQL                     | Azure Cosmos DB        | DynamoDB                | Firestore       |
| Vector Database                 | Azure Cosmos DB for MongoDB |Amazon OpenSearch Serverless  | AlloyDB + pgvector |
| Serviços de IA cognitivos       | Azure OpenAI Service   | Amazon Bedrock          | Vertex AI              |
| CDN                             | Azure Front Door       | CloudFront + AWS WAF    | Cloud CDN + Cloud Armor |
| Mensageria/Filas                | Azure Event Hubs / Service Bus | SNS/SQS/EventBridge/Kinesis | Pub/Sub     |
| Observabilidade (logs/métricas) | Azure Monitor / Application Insights | CloudWatch / X-Ray | Cloud Monitoring / Trace |

4. **Diagrama** — feito no Excalidraw (excalidraw.com), draw.io (diagrams.net) ou à mão fotografado. **Tudo sem instalação.**
Em anexo
> **Entrega:** O grupo commita o esboço (`respostas-aula01.md` + imagem do diagrama) no fork até 1 dia antes da Aula 2. O professor dá feedback escrito durante a semana. O diagrama evolui a cada aula até a entrega final na Aula 6.

---

### Exercício 2.2 — Comparativo de custos: 3 provedores

Você precisa recomendar infraestrutura para um projeto de AI Engineering. Use as calculadoras para comparar:

- 2 VMs com 2 vCPUs e 8 GB RAM (Linux, 24/7)
- 500 GB de object storage
- 1 banco gerenciado com 2 vCPUs / 8 GB RAM / 100 GB
- 10 milhões de requisições/mês para função serverless

| Item               | Azure     | AWS        | GCP         | Notas |
|--------------------|-----------|------------|-------------|-------|
| 2 × VM (2vCPU/8GB) |  U$147,46 | $140,16    | $141,78     | Tipo: |
| 500 GB storage     |  U$9,00   | $11,50     | $10,00      | Tipo: |
| Banco gerenciado   |  U$266,68 | $129,94    | $101,17     | Tipo: |
| 10M req serverless |  U$1,80   | $1,80      | $3,20       | Tipo: |
| **Total mensal**   |  U$425,94 | $283,40    | $256,15     |       |
| **Total anual**    |  U$5111,28| $3.400,80  | $3.073,80   |       |

**Análise:**

a) Qual provedor ficou mais barato? A diferença é significativa?

GCP foi o mais barato, seguido de AWS, com Azure em último lugar. 
A diferença entre GCP e AWS é pequena 10%, praticamente empate técnico considerando variações regionais. Ambos são escolhas viáveis. Azure 66% mais caro que GCP no pay-as-you-go.
b) Aplicando Reserved Instances de 1 ano no mais caro, o resultado muda?
Sim. 
| Provedor | Mensal (reservado 1 ano) | Anual      |
| -------- | ------------------------ | ---------- |
| GCP      | U$178,00 (CUD 1 ano)     | U$2.136,00 |
| AWS      | U$185,00 (RI 1 ano)      | U$2.220,00 |
| Azure    | U$267,55                 | U$3.210,60 |

c) Além de preço, que outros fatores você consideraria para um projeto de IA?

**Calculadoras:**

- Azure: https://azure.microsoft.com/pricing/calculator
- AWS: https://calculator.aws
- GCP: https://cloud.google.com/products/calculator

---

### Exercício 2.3 — Estratégia de migração para sua empresa

Pense no seu contexto profissional atual (ou empresa que conhece bem).

a) Descreva um sistema/workload (sem dados confidenciais — pode ser genérico)
A empresa possui um sistema web interno utilizado para gestão de clientes, pedidos e acompanhamento operacional.

Atualmente o sistema está hospedado em servidores locais, utilizando máquinas virtuais Windows Server, banco de dados SQL Server e armazenamento local para documentos e relatórios.

O sistema é utilizado diariamente pelos colaboradores e possui crescimento constante no volume de dados e acessos.


b) Qual dos 6 Rs você aplicaria? Justifique custo, risco, ganho, prazo
A estratégia escolhida seria Replatform. O sistema continuaria utilizando a mesma aplicação principal e o mesmo banco de dados, porém alguns componentes seriam migrados para serviços gerenciados na nuvem.
O objetivo é reduzir a complexidade operacional, melhorar a disponibilidade e permitir escalabilidade sem a necessidade de uma reescrita completa da aplicação.

Custo:
Moderado, pois não exige reescrita completa da aplicação.

Risco:
Baixo a médio, pois a lógica de negócio permanece inalterada.

Ganho:
Maior disponibilidade, backups gerenciados, monitoramento centralizado e escalabilidade.

Prazo:
Curto a médio prazo, permitindo migração gradual.


c) Que serviço Azure usaria? Estimativa mensal?

Os principais serviços utilizados seriam:

- Azure App Service para hospedagem da aplicação.
- Azure SQL Database para o banco de dados relacional.
- Azure Blob Storage para armazenamento de documentos.
- Azure Monitor para observabilidade.
- Microsoft Entra ID para autenticação e controle de acesso.

Estimativa aproximada:

- Azure App Service: US$ 70/mês
- Azure SQL Database: US$ 120/mês
- Azure Blob Storage: US$ 10/mês
- Azure Monitor: US$ 15/mês

Total estimado:
US$ 215/mês

d) Maior obstáculo técnico ou organizacional? Como endereçaria?

O principal obstáculo seria a resistência à mudança e o planejamento da migração sem interrupção dos serviços.
A adoção da nuvem normalmente exige adequação de processos, treinamento da equipe e revisão de procedimentos operacionais.
Para minimizar os riscos, seria realizada uma migração gradual, iniciando por ambientes de homologação e testes, seguida de validações de desempenho e segurança antes da entrada em produção.

---

## 🔴 Nível 3 — Avançado: IaC e Automação

### Exercício 3.1 — Terraform: endurecer a segurança de rede da VM

**Tudo no Cloud Shell — sem instalação.**

O `main.tf` do lab (Atividade 5) já provisiona a VM com VNet + NSG, mas o NSG vem do portal com a regra **SSH liberada para qualquer origem (`*`)** — uma má prática. **Modifique** o código do lab para deixá-lo mais seguro:

1. **Restrinja o SSH (porta 22)** para aceitar conexões **apenas do seu IP público** (`curl ifconfig.me` no Cloud Shell para obter). Use uma variável `meu_ip` em vez de hardcode.
2. **Adicione uma segunda subnet** chamada `subnet-app` com `10.0.2.0/24` na mesma VNet, pensando em isolar a futura camada de aplicação da QC.
3. **Adicione um output** que exponha apenas o IP público da VM (já existe `public_ip_address` — confirme que aparece após o `apply`).
4. Rode `terraform plan` e identifique no diff **exatamente** qual regra do NSG mudou (não deve recriar a VM).

**Critérios:**

- `terraform plan` + `apply` rodam sem erro e **não recriam** a VM (apenas atualizam o NSG/rede)
- `terraform destroy` ao final remove tudo
- Código commitado no repositório privado do grupo

**Dica:** Documentação dos recursos:

- `azurerm_network_security_group` (bloco `security_rule` → `source_address_prefix`)
- `azurerm_subnet`
- variável `variable "meu_ip"` + `${var.meu_ip}/32`

---

### Exercício 3.2 — Bicep equivalente

Pegue o `main.tf` do lab (a VM + rede) e **traduza para Bicep** — ou parta do `template.json` em `aulas/01-fundamentos-iac/template/` e decompile.

**Tudo no Cloud Shell — `bicep` já está instalado.**

1. Crie `main.bicep` em `~/aula01-bicep/` (ou gere com `bicep decompile template.json --outfile main.bicep`)
2. Implemente os mesmos recursos do lab (RG + VNet + Subnet + NSG + IP + NIC + VM Linux Ubuntu 24.04)
3. Faça deploy com:

```bash
# Bicep precisa do RG já existente OU usar subscription scope
az group create --name rg-bicep-aula01 --location eastus2

az deployment group create \
  --resource-group rg-bicep-aula01 \
  --template-file main.bicep \
  --parameters adminPublicKey="$(cat ~/.ssh/id_rsa.pub)"
```

4. Compare os três artefatos lado a lado e responda no README do grupo:
   - Quantas linhas tem cada arquivo (`template.json` ARM × `main.tf` Terraform × `main.bicep`)?
   - Qual ficou mais legível para você?
   - Em que cenário você escolheria Bicep sobre Terraform?

5. **Não esqueça:** `az group delete --name rg-bicep-aula01 --yes --no-wait` ao final.

---

### Exercício 3.3 — Desafio de arquitetura: multi-cloud para a Quantum Commerce

**Contexto:** O CTO da QC quer evitar lock-in e pediu análise multi-cloud.

a) **Desenhe uma arquitetura multi-cloud** com pelo menos 2 provedores. Justifique por que cada workload em cada nuvem.

b) **Identifique 4 desafios principais**: latência entre nuvens, identidade unificada, custos de egress, observabilidade.

c) **Compare 2 ferramentas IaC multi-cloud:**
   - **Terraform** (HashiCorp) — https://www.terraform.io
   - **Pulumi** — https://www.pulumi.com

   Para cada: linguagem, pricing, suporte aos 3 grandes, quando escolher.

d) **Estime custo de egress:** 10 TB/mês entre Azure (Brazil South) e AWS (us-east-1). Consulte tabelas de preço e calcule.

> **Dica avançada:** Pesquise **Azure Arc** e **AWS Outposts** — como se encaixariam na QC?

---

## Critérios de entrega

A entrega é **um ZIP por grupo** (`entrega-grupo-NN-aula01.zip`) no Portal FIAP. Estrutura completa, prazo e dicas de geração do ZIP em [entregas/entrega-01/INSTRUCOES.md](../../entregas/entrega-01/INSTRUCOES.md).

| Item | Obrigatório? | Pontos máximos |
|------|--------------|----------------|
| Cabeçalho do grupo + distribuição do trabalho | ✅ Sim | 1 pt (Critério 4) |
| 🟢 N1 — Exercícios 1.1, 1.2, 1.3, 1.4 respondidos | ✅ Sim | 3 pts (Critério 1) |
| 🟡 N2 — Exercícios 2.1 (com diagrama), 2.2, 2.3 | ✅ Sim | 3 pts (Critério 2) + 2 pts qualidade técnica (Critério 3) |
| 🔴 N3 — Exercícios 3.1, 3.2, 3.3 (código Terraform/Bicep + README) | 🎁 Bônus | até +2 pts extras |
| Reflexão coletiva ao final | ✅ Sim | 1 pt (Critério 5) |
| **Total da entrega** | | **10 pts** (10% da nota final) |

**Prazo:** 1 dia antes da Aula 2.
**Onde:** upload do ZIP no Portal FIAP. Apenas 1 membro do grupo faz o upload.

> **Atraso:** -1 pt por dia. **Sem entrega:** zero. **Free riders:** quem não contribuir em 2+ aulas consecutivas perde participação no projeto (ver [rubrica](../../entregas/rubrica.md)).

---

## Gabarito parcial — Nível 1

### 1.2 — Os 6 Rs

<details>
<summary>Clique para ver as respostas</summary>

**Cenário A (sistema de 2008 sem doc):** **Rehost (Lift & Shift)** — migração rápida sem alterar código. Risco do único mantenedor + urgência justificam o caminho de menor esforço.

**Cenário B (ERP com 5 usuários/mês):** **Retire** — o custo de migrar provavelmente excede o valor gerado. Arquivar dados em storage frio e aposentar.

**Cenário C (API de pagamentos monolítica):** **Refactor** — decisão de reescrever em microserviços foi explícita. Alto esforço, maior ganho.

**Cenário D (CRM interno de 15 anos):** **Repurchase** — SaaS de mercado cobre 90% por menor custo. Decisão de negócio.

**Cenário E (mainframe por regulação BACEN):** **Retain** — compliance dita. Não é falha de estratégia.

</details>

### 1.3 — SLA e impacto

<details>
<summary>Clique para ver as respostas</summary>

**a)** 8.760 × (1 - 0,999) = **8,76 horas/ano** (~525 min)

**b)** 8,76 × R$ 50.000 = **R$ 438.000/ano**

**c)** Máx downtime = 50.000 ÷ 50.000 = 1h/ano → 1 ÷ 8.760 = 0,0114% → SLA mínimo **99,9886%** → na prática **SLA 99,99%** (52 min/ano)

</details>


